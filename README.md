# 📊 Stock Screener v2

หน้าเว็บรายงานหุ้นเด็ดทุกวัน ออกแบบสำหรับมือใหม่ — ภาษาไทย เข้าใจง่าย มี emoji 🌸

🌐 **เว็บไซต์:** https://[your-username].github.io/Stock_filter_helper/

---

## 🎨 จุดเด่น Design v2

- 🌸 **Style สดใส** — เหมือน Shopee/Lazada ไม่น่าเบื่อแบบรายงานการเงิน
- 🇹🇭 **ภาษาไทยทั้งหมด** — ไม่มีศัพท์ยาก อธิบายเหมือนคุยกับเพื่อน
- 😀 **Emoji ทุกที่** — แต่ละหุ้นมี emoji เฉพาะตัว, ข่าวมี emoji, ตัวเลขมี emoji
- ⭐ **Score Bar สีไล่** — เห็นปุ๊บรู้ปั๊บว่าหุ้นไหนดี
- 📈 **กราฟใหญ่ 20 วัน** + Volume bar + RSI gauge
- 🥇🥈🥉 **เหรียญรางวัล** อันดับ 1-3 เด่นพิเศษ
- 👍👎 **Pros/Cons แยกชัด** ในกล่องสีเขียว-แดง
- 🌙 **Dark mode อัตโนมัติ**

---

## 🚀 Deploy ครั้งแรก

### 1. Push ขึ้น GitHub repo (ใช้ repo เดิมได้)

```bash
cd Stock_filter_helper
# แทนที่ไฟล์เก่าด้วย v2
# (เก็บไฟล์เก่าไว้ก็ได้ใน archive/)

git add .
git commit -m "Update to v2: friendly Thai design with indicators"
git push origin main
```

### 2. แก้ Routine prompt

ไปที่ **claude.ai/code/routines** → routine ของคุณ → **Edit**

ลบ prompt เก่าออก → วาง prompt ใหม่จาก `prompts/daily-screener.md`

🆕 **สำคัญที่สุด:** prompt v2 บอก Routine ให้ **push ตรงเข้า main** ไม่สร้าง branch ใหม่อีก

### 3. Run now ทดสอบ

กด Run → รอ 5-10 นาที → ครั้งนี้:
- ✅ Push เข้า main ตรงๆ
- ✅ ไม่ต้อง merge เอง
- ✅ GitHub Pages อัพเดทเอง

---

## ⚙️ ปรับแต่ง

### เปลี่ยนสีหลัก
แก้ `assets/template.html` → section `:root { --pink-strong: ... }`

### เปลี่ยน emoji ของหุ้น
แก้ใน prompt → field `emoji` ของแต่ละ stock — Routine จะใส่ตามที่บอก

### เพิ่ม/ลด indicator
แก้ prompt section "STEP 3" — เพิ่ม MACD, Bollinger Bands, ฯลฯ ได้

---

## 📁 Folder Structure

```
Stock_filter_helper/
├── prompts/
│   ├── daily-screener.md       # Prompt: หุ้นใหญ่ (default theme)
│   └── daily-smallcap.md       # Prompt: หุ้นเล็กดาวรุ่ง ราคา < $20 (smallcap theme)
├── docs/                        # ⭐ GitHub Pages serve จากที่นี่
│   ├── index.html               # หน้าหุ้นใหญ่ (รายงานวันล่าสุด)
│   ├── archive/                 # ประวัติของหุ้นใหญ่
│   │   ├── index.html
│   │   ├── index.json
│   │   └── YYYY-MM-DD.html
│   └── smallcap/                # 🌱 section หุ้นเล็กดาวรุ่ง
│       ├── index.html           # หน้าหุ้นเล็กวันล่าสุด
│       └── archive/             # ประวัติของหุ้นเล็ก (แยก index.json เป็นของตัวเอง)
│           ├── index.html
│           ├── index.json
│           └── YYYY-MM-DD.html
├── assets/
│   └── template.html            # Template ใช้ร่วมกัน 2 routine — เปลี่ยน theme ผ่าน REPORT_THEME
└── README.md
```

## 🌱 Two Routines

repo นี้รองรับ 2 routines ใน claude.ai/code/routines (แยกกันชัด):

| Routine | Prompt | Output | Theme |
|---------|--------|--------|-------|
| หุ้นใหญ่ | `prompts/daily-screener.md` | `docs/index.html` | default (ส้ม-ชมพู) |
| หุ้นเล็กดาวรุ่ง | `prompts/daily-smallcap.md` | `docs/smallcap/index.html` | smallcap (น้ำเงิน-ม่วง) + extra disclaimer |

ทั้ง 2 routines ใช้ `assets/template.html` ตัวเดียวกัน ต่างกันที่ `REPORT_THEME` constant ในส่วน routine block

---

## ⚠️ Disclaimer

This is **information only — not investment advice.**  
ข้อมูลจัดทำเพื่อให้ความรู้เท่านั้น ไม่ใช่คำแนะนำลงทุน  
หุ้นมีความเสี่ยง ผู้ลงทุนควรศึกษาข้อมูลก่อนตัดสินใจ
