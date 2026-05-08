# Stock Screener

รายงานหุ้น US (S&P 500 + Nasdaq) ที่กรองด้วย AI ทุกเช้าวันทำการ — แสดง Top 10 พร้อม fundamental, sentiment ข่าว, และ technical reference levels

🌐 **Live site:** https://[your-username].github.io/stock-screener/

---

## ⚠️ Disclaimer

This is **information only — not investment advice.**
ข้อมูลทั้งหมดเป็นการรวบรวมจากแหล่งสาธารณะ (Yahoo Finance, Reuters, MarketWatch, etc.) เพื่อใช้ประกอบการตัดสินใจของผู้ใช้เท่านั้น การตัดสินใจซื้อ-ขาย และความเสี่ยงทั้งหมดเป็นของผู้ใช้

---

## โครงสร้าง Repo

```
stock-screener/
├── prompts/
│   └── daily-screener.md      # Prompt ของ Routine
├── docs/
│   └── index.html              # หน้าเว็บปัจจุบัน (GitHub Pages serve จากนี่)
├── archive/
│   ├── index.html              # หน้ารายการประวัติ
│   ├── index.json              # ดัชนีรายงานย้อนหลัง
│   └── YYYY-MM-DD.html         # รายงานย้อนหลัง
├── assets/
│   └── template.html           # Template ที่ Routine ใช้สร้าง report
└── README.md
```

---

## ขั้นตอน Deploy ครั้งแรก

### 1. สร้าง GitHub repo

```bash
cd /path/to/stock-screener
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/stock-screener.git
git push -u origin main
```

### 2. เปิด GitHub Pages

1. ไปที่ repo → **Settings** → **Pages**
2. **Source:** Deploy from a branch
3. **Branch:** `main` / **Folder:** `/docs`
4. Save

รอ 1-2 นาที จะได้ URL: `https://YOUR_USERNAME.github.io/stock-screener/`

### 3. ตั้งค่า Claude Routine

ไปที่ **claude.ai/code/routines** → **New routine**:

| Field | ค่า |
|-------|-----|
| Name | `Daily Stock Screener` |
| Trigger | Scheduled |
| Schedule | Weekdays at 07:00 Asia/Bangkok |
| Environment | Remote |
| Repository | `YOUR_USERNAME/stock-screener` (ให้ branch push permission) |
| Connectors | (ไม่ต้องเลือก — ใช้ web_search ในตัว) |

**Prompt:** copy เนื้อหาจาก `prompts/daily-screener.md` ทั้งหมดวางลงในช่อง prompt ของ Routine

### 4. Test run

กด **Run now** → รอ 3-5 นาที → เช็ค:
- มี commit ใหม่ใน repo
- `docs/index.html` ถูก update
- เว็บที่ deploy แสดง 10 หุ้น

---

## วิธีปรับเกณฑ์ screening

แก้ไข `prompts/daily-screener.md` section **Step 1** เปลี่ยน threshold ของ metric ตามที่ต้องการ เช่น:

- เน้นหุ้นเติบโต → ลด P/E threshold ลง, เพิ่ม Revenue growth threshold
- เน้นหุ้น dividend → เพิ่มเกณฑ์ dividend yield > 2%
- เน้น mid cap → ลด market cap threshold เหลือ $2B-$10B

หลังแก้ commit + push → Routine จะใช้เกณฑ์ใหม่ในรอบถัดไป

---

## Troubleshooting

**Routine ทำงานแล้วแต่ docs/index.html ไม่ update:**
- เช็ค repository permissions ว่ามี branch push ไหม
- เช็ค Routine logs ที่ claude.ai/code/routines

**GitHub Pages ไม่ขึ้น:**
- รอ 5-10 นาทีหลัง push (deploy ใช้เวลา)
- เช็คใน Settings → Pages ว่า source ถูกต้อง

**ข้อมูลหุ้นไม่ตรง real-time:**
- Yahoo Finance ฟรี delay ~15-20 นาที — เป็นเรื่องปกติ
- Routine ออกแบบให้รัน pre-market ก่อนตลาดเปิด ใช้ข้อมูลปิดของวันก่อน

**Run quota หมด:**
- Pro plan = 5 routine runs/วัน
- ถ้ารัน weekdays only ใช้แค่ 1/วัน เหลืออีก 4 ไว้ on-demand
