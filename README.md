# Org Structure Studio

Web application (ไฟล์เดียว `index.html`) สำหรับออกแบบ จัดเก็บ และเปรียบเทียบโครงสร้างองค์กรของกลุ่มบริษัท
PCC, PDE, PPP, SBP, PCE, PSL, PEM, PEMC, PSP

> สถานะ: **Prototype** — ข้อมูลที่ติดมากับหน้าเว็บเป็น *ข้อมูลตัวอย่าง* ยังไม่ใช่โครงสร้างจริง

## ความสามารถหลัก

| ความต้องการ | สิ่งที่มีใน Prototype |
| --- | --- |
| 1. เก็บ Inactive Version อันเก่า | เมื่อ Publish Draft เป็น Active ระบบย้าย Active เดิมเป็น **Inactive** อัตโนมัติ พร้อมวันสิ้นสุด (ก่อนวันมีผล 1 วัน) เปิดดูย้อนหลัง เปรียบเทียบ และสร้าง Draft ใหม่จากเวอร์ชันเก่าได้ |
| 2. เก็บ Draft Version | สร้าง Draft ได้หลายฉบับต่อบริษัท (คัดลอกจากเวอร์ชันเดิม / แม่แบบมาตรฐาน / แม่แบบ Platform Organization / โครงเปล่า) |
| 3. แก้ไขบนหน้าเว็บ | คลิกเลือก ดับเบิลคลิกเปลี่ยนชื่อ ลากวางเพื่อย้ายสังกัด แก้รายละเอียดที่แผงขวา แท็บ “รายการ” พิมพ์โครงสร้างแบบ outline ได้เร็ว มี Undo/Redo |
| 4. เครื่องมือสร้างโครงสร้างตามเอกสาร | แผนผังแบบลำดับชั้น (คณะกรรมการ › ผู้บริหาร › สายงาน › ฝ่าย › แผนก) หน่วยงานขึ้นตรง (Staff) วางด้านข้าง และ **สร้างจากข้อความ** วางรายชื่อหน่วยงานจาก Word / PowerPoint / PDF / Excel แล้วระบบแปลงเป็นแผนผัง |
| 5. Login ด้วย GitHub Token | ผู้มีสิทธิ์แก้ไขเข้าสู่ระบบด้วย Fine-grained Personal Access Token ทุกครั้งที่กด “บันทึก” จะเป็น commit ไปที่ `data/org-structure.json` ผู้ไม่ได้เข้าสู่ระบบดูได้อย่างเดียว |

ความสามารถเสริมที่สอดคล้องกับการเปลี่ยนผ่านสู่ Agentic AI Organization:

- ประเภทหน่วยงาน **AI Agent** (พร้อมผู้ดูแล human-in-the-loop) **แพลตฟอร์ม** และ **พันธมิตร (Ecosystem)**
- ระบุรูปแบบองค์กรของแต่ละเวอร์ชัน: Industrial → Platform → Ecosystem
- ตัวชี้วัดโครงสร้าง: จำนวนหน่วยงาน อัตรากำลัง ชั้นการบังคับบัญชา Span of control จำนวน AI Agent
- เปรียบเทียบสองเวอร์ชัน: หน่วยงานใหม่ / ยุบ / ย้ายสังกัด / เปลี่ยนชื่อ และผลต่างอัตรากำลัง
- Export เป็น PNG, CSV (เปิดใน Excel ภาษาไทยได้), JSON

## เปิดใช้งาน

**ทดลองในเครื่อง:** ดับเบิลคลิก `index.html` ได้เลย (ใช้ข้อมูลตัวอย่าง แก้ไขได้ในเบราว์เซอร์)

**ใช้งานจริงผ่าน GitHub Pages:**

1. Repository › Settings › Pages
2. Source: *Deploy from a branch* › Branch `main` / `(root)` › Save
3. เปิด `https://<owner>.github.io/<repo>/`

> หมายเหตุ: GitHub Pages ของ repository ส่วนตัวต้องใช้แพ็กเกจ GitHub แบบเสียเงิน และไฟล์บน Pages เปิดดูได้ตาม URL
> ถ้าโครงสร้างองค์กรเป็นข้อมูลภายใน ให้ตั้ง Pages เป็น private (GitHub Enterprise) หรือเปิดไฟล์จากเครื่องแทน

## สิทธิ์การแก้ไข (GitHub Token)

1. เปิด GitHub › Settings › Developer settings › **Fine-grained tokens** › Generate new token
2. Repository access: **Only select repositories** › เลือก repository นี้
3. Repository permissions › **Contents: Read and write**
4. คัดลอก Token ไปวางในหน้า “เข้าสู่ระบบ”

Token เก็บใน sessionStorage ของเบราว์เซอร์ (หายเมื่อปิดแท็บ) เว้นแต่เลือก “จดจำในเบราว์เซอร์นี้”
และส่งไปที่ `api.github.com` เท่านั้น ผู้ที่จะแก้ไขต้องเป็น collaborator ที่มีสิทธิ์ write ใน repository

## โครงสร้างข้อมูล

ทุกบริษัทและทุกเวอร์ชันอยู่ในไฟล์เดียว `data/org-structure.json` (สร้างให้อัตโนมัติเมื่อบันทึกครั้งแรก)

```jsonc
{
  "schema": 1,
  "companies": [{ "code": "PCC", "nameTh": "", "nameEn": "" }],
  "versions": [{
    "id": "v_…", "companyCode": "PCC", "versionNo": 2,
    "name": "โครงสร้างองค์กร พ.ศ. 2569",
    "status": "active",            // active | draft | inactive
    "model": "industrial",         // industrial | platform | ecosystem
    "effectiveDate": "2026-01-01", "endDate": null, "plannedDate": "",
    "approvalRef": "มติคณะกรรมการบริษัท ครั้งที่ 6/2568",
    "basedOn": "v_…",
    "root": {
      "id": "n_…", "type": "board", "name": "คณะกรรมการบริษัท",
      "nameEn": "", "code": "", "title": "", "head": "", "hc": 0,
      "staff": false, "layout": "auto", "note": "",
      "children": []
    }
  }]
}
```

`type` ของหน่วยงาน: `board` `committee` `executive` `line` `office` `platform` `division` `department` `section` `team` `position` `ai` `partner`

หน่วยงานที่คัดลอกมาจากเวอร์ชันก่อนหน้าใช้ `id` เดิม ระบบจึงเปรียบเทียบได้ว่าหน่วยใดย้ายสังกัดหรือเปลี่ยนชื่อ

## รูปแบบข้อความที่ “สร้างจากข้อความ” รองรับ

```text
คณะกรรมการบริษัท
  คณะกรรมการตรวจสอบ (staff)
  กรรมการผู้จัดการ | กรรมการผู้จัดการ | | 1
    ฝ่ายขายและการตลาด | ผู้อำนวยการฝ่าย | | 2
      แผนกขายโครงการ | ผู้จัดการแผนก | | 12
    ฝ่ายทรัพยากรบุคคล
      HR Service Agent [AI]
```

- ระดับจากการเยื้องบรรทัด (Tab / เว้นวรรค) หรือเลขลำดับ `1.` `1.1` `1.1.1`
- วางจาก Excel: คอลัมน์ ระดับ | ชื่อหน่วยงาน | ประเภท | ตำแหน่งหัวหน้า | ผู้ดำรงตำแหน่ง | อัตรากำลัง
- ข้อมูลเพิ่มในบรรทัด: `ชื่อ | ตำแหน่งหัวหน้า | ผู้ดำรงตำแหน่ง | อัตรากำลัง`
- `[ฝ่าย]` `[แผนก]` `[AI]` ระบุประเภท, `(staff)` หน่วยงานขึ้นตรง หากไม่ระบุ ระบบเดาจากคำนำหน้า (ฝ่าย / แผนก / สำนัก / สายงาน …)
