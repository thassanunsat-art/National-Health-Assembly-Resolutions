# แดชบอร์ดมติสมัชชาสุขภาพแห่งชาติ · NHA Resolutions Dashboard

แดชบอร์ดแบบอินเทอร์แอคทีฟสำหรับติดตามการขับเคลื่อน **มติสมัชชาสุขภาพแห่งชาติ (NHA)**
พ.ศ. 2551–2568 — เว็บไซต์แบบ static ล้วน ไม่มี backend ไม่มี build step ฝั่งหน้าเว็บ
และไม่มี dependency ภายนอกตอน runtime (กราฟทั้งหมดวาดด้วย SVG ที่เขียนเอง)

An interactive dashboard tracking Thailand's National Health Assembly resolutions
and how they have been driven forward. Pure static site — no backend, no runtime
dependencies, all charts hand-built in SVG.

---

## 📊 มุมมองในแดชบอร์ด · Views

| มุมมอง | เนื้อหา |
|---|---|
| **ภาพรวม** · Overview | KPI, สัดส่วนสถานะการขับเคลื่อน, จำนวนมติรายปี, องค์ประกอบสถานะรายปี, ตารางมติทั้งหมด |
| **กลุ่มมติ & ประเด็น** · Groups & Issues | มติแยกตามกลุ่ม (stacked ตามสถานะ), ประเภทการขับเคลื่อน, มติที่ขับเคลื่อนเข้มข้นที่สุด, heatmap กลุ่มมติ × ประเภทงาน |
| **หน่วยงานขับเคลื่อน** · Organizations | สัดส่วนภาคส่วน, heatmap ภาคส่วน × ประเภทงาน, 20 หน่วยงานที่ขับเคลื่อนมากที่สุด, ทะเบียนหน่วยงาน |
| **มติ ครม.** · Cabinet | สถานะการเสนอเข้า ครม., มติ ครม. รายปี, ไทม์ไลน์มติคณะรัฐมนตรีพร้อมรายละเอียด |

ฟีเจอร์เสริม: **ลิงก์โฟลเดอร์เอกสารรายมติ**, **สลับภาษาไทย/อังกฤษ**, ตัวกรองร่วมทุกมุมมอง (ปี / กลุ่มมติ / สถานะ / ภาคส่วน / ค้นหาข้อความ),
ตารางเรียงลำดับได้, แผงรายละเอียด (drawer) เมื่อคลิกมติ–หน่วยงาน–มติ ครม.,
tooltip บนทุกกราฟ, โหมดสว่าง/มืด, รองรับมือถือ และสั่งพิมพ์ได้

---

## 📎 ลิงก์เอกสารประกอบมติ · Document links

ดึงจาก `data/source/Cabinet-resolution-documents.docx` (คอลัมน์ "ไฟล์เอกสารแนบ")
ตอน build โดยอัตโนมัติ — เป็นลิงก์โฟลเดอร์ Google Drive

ลิงก์ในไฟล์ Word มี 2 ระดับ ETL แยกให้เอง:

| ระดับ | ป้ายในไฟล์ Word | ผลลัพธ์ |
|---|---|---|
| เฉพาะมติ | `๑.๒ การเข้าถึงยาถ้วนหน้า…` | ผูกกับ **มติ 1.2** เท่านั้น |
| ทั้งสมัชชา | `NHA ๘`, `NHA ๖-๗` | ผูกกับ **ทุกมติ**ของสมัชชาครั้งนั้น |

ลิงก์เฉพาะมติมาก่อนเสมอ ถ้าไม่มีจึงใช้ลิงก์ระดับสมัชชาแทน ผลลัพธ์เก็บใน
`resolutions[].docLink` และ `resolutions[].docScope` (`resolution` / `assembly`)

**ความครอบคลุมปัจจุบัน: 65 จาก 103 มติ** (เฉพาะมติ 17 · ระดับสมัชชา 48 ·
ไม่มีลิงก์ 38) — ที่ยังไม่มีคือบางมติของสมัชชาครั้งที่ 1, 2, 3, 5
และทั้งหมดของครั้งที่ 16, 17, 18 เพราะไฟล์ Word ต้นทางยังไม่มีลิงก์ให้

เพิ่มลิงก์ภายหลัง: แก้ไฟล์ Word ให้คอลัมน์ "ไฟล์เอกสารแนบ" มี hyperlink
โดยตั้งข้อความเป็นเลขมติ (เช่น `๑๖.๑ …`) หรือ `NHA ๑๖` แล้วรัน `build_data.py` ใหม่
ไม่ต้องแก้โค้ด

แสดงผล 3 จุด: KPI "มติที่มีเอกสารแนบ" ในแท็บภาพรวม · คอลัมน์ "เอกสาร"
ในตารางมติ (ไอคอนโฟลเดอร์ มีเลขครั้งกำกับเมื่อเป็นลิงก์ระดับสมัชชา) ·
ปุ่มในแผงรายละเอียดพร้อมบอกว่าเป็นโฟลเดอร์เฉพาะมติหรือของทั้งสมัชชา

---

## 🌐 ภาษา · Language

ปุ่ม **ไทย / EN** ที่มุมขวาบนสลับภาษาทั้งหน้า และจำค่าที่เลือกไว้ใน `localStorage`
ค่าเริ่มต้นคือภาษาไทย

สิ่งที่แปล: ข้อความ UI ทั้งหมด (หัวข้อ, การ์ด, ตัวกรอง, หัวตาราง, tooltip, แผงรายละเอียด)
และ**คำศัพท์ประจำมิติ** ได้แก่ กลุ่มมติ 16 กลุ่ม, ประเภทการขับเคลื่อน 10 ประเภท,
ภาคส่วน 5 ภาคส่วน, สถานะการขับเคลื่อน 5 สถานะ และสถานะการเสนอ ครม.

สิ่งที่**ไม่แปล**: ชื่อมติ ชื่อหน่วยงาน และเนื้อความมติ ครม. เพราะต้นทางมีเฉพาะภาษาไทย
จึงแสดงตามต้นฉบับทั้งสองภาษา ส่วนปีใช้ พ.ศ. ตามต้นทางเสมอ โดยโหมดอังกฤษกำกับว่า `B.E.`
แทนการแปลงเป็น ค.ศ. เพื่อไม่ให้ตัวเลขเปลี่ยนไปมาระหว่างภาษา

แก้คำแปลได้ที่ `assets/i18n.js` ไฟล์เดียว (`STR` = ข้อความ UI, `GROUP_EN` / `ISSUE_EN` /
`SECTOR_EN` / `CAB_LABEL` / `STATUS_LABEL` = คำศัพท์ประจำมิติ)

---

## 🚀 Deploy บน GitHub Pages

1. สร้าง repository ใหม่บน GitHub แล้ว push โฟลเดอร์นี้ขึ้นไป

   ```bash
   git init -b main
   git add .
   git commit -m "NHA resolutions dashboard"
   git remote add origin https://github.com/<user>/<repo>.git
   git push -u origin main
   ```

2. ที่หน้า repo → **Settings → Pages → Build and deployment → Source** เลือก
   **GitHub Actions**

3. workflow `.github/workflows/deploy.yml` จะทำงานอัตโนมัติทุกครั้งที่ push เข้า `main`
   โดยจะ rebuild `data/nha_data.json` จากไฟล์ Excel ต้นทาง → ตรวจความถูกต้อง → deploy

4. เว็บจะออนไลน์ที่ `https://<user>.github.io/<repo>/`

> ถ้า Actions ยังไม่รัน ให้ไปที่แท็บ **Actions** แล้วกด **Run workflow** ด้วยตัวเอง (workflow_dispatch)

---

## 🛠 การพัฒนาในเครื่อง · Local development

ต้องเสิร์ฟผ่าน HTTP (ไฟล์ JSON โหลดผ่าน `fetch` จึงเปิดแบบ `file://` ไม่ได้)

```bash
python3 -m http.server 8000
# เปิด http://localhost:8000
```

### อัปเดตข้อมูล · Updating the data

แก้ไฟล์ Excel ใน `data/source/` แล้วรัน:

```bash
pip install pandas openpyxl
python scripts/build_data.py    # สร้าง data/nha_data.json ใหม่
python scripts/check_data.py    # ตรวจความครบถ้วน/ความสอดคล้อง
```

commit แล้ว push — Actions จะ deploy ให้เอง

---

## 📁 โครงสร้างโปรเจกต์ · Project structure

```
.
├── index.html                    # โครงหน้าเว็บ + tabs + ตัวกรอง
├── assets/
│   ├── styles.css                # design tokens, light/dark, layout
│   ├── i18n.js                   # คำแปลไทย/อังกฤษทั้งหมด (แก้ที่นี่ที่เดียว)
│   ├── app.js                    # ตัวกรอง, กราฟ SVG, ตาราง, drawer
│   ├── nhco-mark-180.png         # ตราสัญลักษณ์ สช. (หัวเว็บ)
│   ├── nhco-mark-64.png          # favicon
│   └── nhco-logo.png             # โลโก้เต็มพร้อมชื่อหน่วยงาน (ท้ายเว็บ)
├── data/
│   ├── nha_data.json             # payload ที่แดชบอร์ดใช้ (generated — อย่าแก้มือ)
│   └── source/                   # ไฟล์ Excel ต้นทาง
│       ├── NHA_1.xlsx
│       ├── Cabinet-resolution_NHCO.xlsx
│       ├── FACT_Cabinet-resolution.xlsx
│       ├── IHA.xlsx
│       ├── datadic_NHA.xlsx
│       └── Cabinet-resolution-documents.docx   # ลิงก์โฟลเดอร์เอกสารรายมติ
├── scripts/
│   ├── build_data.py             # ETL: Excel → JSON
│   └── check_data.py             # การตรวจสอบความถูกต้องสำหรับ CI
└── .github/workflows/deploy.yml  # build + deploy ไป GitHub Pages
```

---

## 🗃 แบบจำลองข้อมูล · Data model

`data/nha_data.json` ประกอบด้วย

| คีย์ | คำอธิบาย |
|---|---|
| `meta` | ชื่อชุดข้อมูล แหล่งที่มา วันที่ generate และจำนวนรวม |
| `resolutions[]` | มติสมัชชาฯ — `id`, `title`, `group`, `year`, `source`, `status`, `cabinetStatus`, `drivenCount`, `orgCount`, `docLink`, `docScope` |
| `actions[]` | กิจกรรมขับเคลื่อน — `resId`, `org`, `orgGroup` (ภาคส่วน), `issue` (ประเภทงาน), `detail` |
| `cabinet[]` | มติคณะรัฐมนตรี — `id`, `date`, `title`, `detail`, `link`, `resolutions[]` |
| `cabinetLinks[]` | ความสัมพันธ์ มติสมัชชาฯ ↔ มติ ครม. พร้อมสถานะการเสนอ |
| `iha[]` | สมัชชาสุขภาพเฉพาะประเด็น (Issue-based Health Assembly) |
| `dims` | ค่าที่เป็นไปได้ของแต่ละมิติ ใช้สร้างตัวกรอง |

**สถานะการขับเคลื่อน** (`DIM_status_NHA`): `On-going`, `Achieved`, `To be revisited`,
`To find key mechanism`, `End-up`

**ประเภทการขับเคลื่อน** (`DIM_driven_issues_NHA`): ข้อกฎหมาย/ประกาศคำสั่ง ·
จัดทำยุทธศาสตร์/แผนแม่บท · กิจกรรมโครงการ · พัฒนาระบบและกลไก · รูปธรรมในระดับพื้นที่ ·
ระบบข้อมูล · นโยบาย · องค์ความรู้/งานวิจัย · สนับสนุนงบประมาณ/อุปกรณ์

---

## 🎨 หมายเหตุด้านการออกแบบ · Design notes

- โลโก้ สช. วางบนพื้นขาวเสมอ เพราะตัวโลโก้เป็นสีม่วงเข้ม จะจมหากวางบนพื้นโหมดมืดโดยตรง
- ชุดสีผ่านการตรวจ colorblind-safety แล้ว (CVD ΔE ≥ 8, normal-vision ΔE ≥ 15) ทั้งโหมดสว่างและมืด
  โดยโหมดมืดเลือกค่าสีแยกต่างหาก ไม่ใช่การกลับสีอัตโนมัติ
- สีบอกสถานะมีข้อความกำกับเสมอ (legend + direct label) สีจึงไม่ใช่ช่องทางเดียวในการสื่อความหมาย
- แกนและเส้นกริดถูกลดความเด่น, ปลายแท่งกราฟโค้ง 4px, มีช่องว่าง 2px ระหว่าง segment ใน stacked bar
- ค่าตัวเลขและป้ายกำกับใช้สี text token ไม่ใช่สีของ series

## 📄 แหล่งข้อมูล · Data source

สำนักงานคณะกรรมการสุขภาพแห่งชาติ (สช.) — National Health Commission Office (NHCO)
ชุดข้อมูล `Name_NHA`, `driven_NHA`, `Cabinet-resolution_NHCO`
