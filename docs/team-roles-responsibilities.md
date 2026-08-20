# Team Roles & Responsibilities

> **ที่มา:** วิเคราะห์จากสไลด์ Functional Requirements ในโฟลเดอร์ `img/`
> **ขอบเขต:** เอกสารนี้อธิบายว่าแต่ละตำแหน่งทำอะไร **ในระบบนี้** — ทั้งในฐานะผู้ใช้ระบบ และในฐานะคนที่ถูกวัด KPI จากระบบ

---

## สรุปสั้น

ระบบนี้ระบุไว้ชัด 6 role: **SA, UX/UI, Dev, Tester (QA), PM, HR** โดยแต่ละ role มี:

1. **Conditional Fields ของตัวเอง** ใน Task form (Module 02) — ข้อมูลที่ต้องกรอกไม่เหมือนกัน
2. **Defect Type ที่เป็นความรับผิดชอบของตัวเอง** (Module 03)
3. **KPI Metric ของตัวเอง** (Module 05) — วัดด้วยตัวเลขต่างกัน

โครงสร้างนี้บอกอย่างชัดเจนว่า **ระบบถูกออกแบบมาเพื่อกระจายความรับผิดชอบด้านคุณภาพให้ทุก role ไม่ใช่กองไว้ที่ Dev**

---

## ตารางภาพรวม

| Role | Conditional Fields (M02) | Defect Type ที่รับผิด (M03) | KPI Metric หลัก (M05) |
|------|--------------------------|---------------------------|----------------------|
| **SA** | Deliverable, Approval | SA Gap | Gap Rate |
| **UX/UI** | Figma link, Revision count | Design Gap | Revision Rate |
| **Dev** | *(งาน implement)* | Code Bug | Bug Rate |
| **Tester (QA)** | Pass/Fail Count | Test Escape | Escape Rate |
| **PM** | *(ผู้ใช้ dashboard)* | — (เป็นผู้ Acknowledge) | ให้คะแนน 20% |
| **HR** | — | — | ให้คะแนน 10% |

---

## 1. SA — System Analyst

### หน้าที่ในระบบ

**ต้นน้ำที่สุดของสายงาน** — SA เป็นคนแปลงความต้องการของลูกค้าเป็น Requirement ที่ทีมทำต่อได้

**งานที่ทำในระบบ:**
- สร้างและดูแล **Requirement** (Module 01) — ทั้ง Functional และ NFR
- ระบุ **Req Category** (Functional / Non-Functional) ให้ถูกต้อง
- กรอก **NFR Metric** ที่วัดได้เป็นตัวเลข — ระบบบังคับ ไม่ให้เขียน "ต้องเร็ว" ลอยๆ
- กำหนด **MoSCoW Priority** ร่วมกับ PM
- ใส่ **Initial Estimate** ระดับ Requirement
- ผูก Requirement เข้ากับ **Module + Sprint**

**Conditional Fields ที่ SA ต้องกรอกใน Task:**
| Field | ความหมาย |
|-------|----------|
| **Deliverable** | ส่งมอบอะไร (spec doc, flow diagram, data dictionary) |
| **Approval** | ใครอนุมัติแล้วหรือยัง — เอกสารที่ยังไม่ approve ถือว่างานยังไม่จบ |

### KPI ที่ใช้วัด SA

**Gap Rate** — สัดส่วน Defect ประเภท `SA Gap` ต่อจำนวน Requirement ที่เขียน

`SA Gap` = ปัญหาที่เกิดเพราะสเปคไม่ครบ ไม่ชัด หรือขัดแย้งกันเอง เจอตอน dev หรือตอน test

**นัยของ metric นี้:** SA ที่เขียนสเปคเร็วแต่หลุดเยอะ จะเห็นในตัวเลข — ผลักดันให้ SA ลงทุนกับการ review สเปคก่อนส่งต่อ

### ความเสี่ยงของตำแหน่งนี้

- Gap Rate อาจสูงโดยไม่ใช่ความผิด SA — เช่น ลูกค้าเปลี่ยนใจ ซึ่งควรถูกจัดเป็น `Req Change` ใน Delay Cause ไม่ใช่ SA Gap ต้องมีคนตัดสินที่ชัดเจน
- เส้นแบ่งระหว่าง `SA Gap` กับ `Code Bug` คลุมเครือ — dev อาจโยนว่าสเปคไม่บอก, SA อาจบอกว่าเป็นเรื่อง common sense

---

## 2. UX/UI Designer

### หน้าที่ในระบบ

แปลง Requirement เป็นหน้าตาและ flow ที่ใช้งานได้จริง

**งานที่ทำในระบบ:**
- รับ Requirement จาก Module 01 มาออกแบบ
- ส่งมอบงานผ่าน **Figma link** (บันทึกใน Task)
- บันทึก **Revision count** ทุกครั้งที่แก้แบบ

**Conditional Fields ที่ UX ต้องกรอกใน Task:**
| Field | ความหมาย |
|-------|----------|
| **Figma** | ลิงก์ไปงานออกแบบ — เป็นหลักฐานการส่งมอบ |
| **Revision** | จำนวนรอบที่แก้ — ตัวเลขนี้ feed เข้า KPI |

### KPI ที่ใช้วัด UX/UI

**Revision Rate** — จำนวนรอบแก้แบบเฉลี่ยต่องาน

**นัยของ metric นี้:** Revision สูงหมายถึงเข้าใจโจทย์ผิดตั้งแต่แรก หรือไม่ได้คุยกับ stakeholder ให้พอก่อนลงมือ

**Design Gap** — Defect ที่เกิดเพราะ design ไม่ครอบ edge case (เช่น ไม่ได้ออกแบบหน้า error, ไม่ได้คิดถึง state ที่ข้อมูลว่าง)

### ความเสี่ยงของตำแหน่งนี้

> ⚠️ **Revision Rate เป็น metric ที่อันตรายที่สุดในระบบนี้**

การแก้แบบไม่ใช่ความล้มเหลวเสมอไป — บางครั้งคือ iteration ที่ดีที่ทำให้งานดีขึ้น ถ้าวัด revision เป็นสิ่งลบ นักออกแบบจะ:
- ไม่ยอมเสนอทางเลือกหลายแบบ
- ไม่กล้าแก้แม้รู้ว่าแบบเดิมไม่ดี
- ป้องกันตัวโดยการ lock แบบเร็วเกินไป

**ข้อเสนอ:** ควรแยก revision เป็น 2 ประเภท
- `Revision (Requirement Change)` — ไม่นับ penalty
- `Revision (Design Miss)` — นับ

หรือกำหนด baseline ที่ยอมรับได้ (เช่น 2 รอบเป็นเรื่องปกติ นับเฉพาะที่เกิน)

---

## 3. Dev — Developer (FE / BE)

### หน้าที่ในระบบ

แปลง Requirement + Design เป็น code ที่ทำงานได้

**งานที่ทำในระบบ:**
- รับ Task ที่แตกมาจาก Requirement
- อัปเดต **Work Pattern** (Sequential / Parallel / Independent) — บอกว่างานนี้ทำคู่ขนานกับใครได้
- ระบุ **Blocked By** เมื่อติดรอคนอื่น → เข้า PM alert view
- กรอก **Days Late + Delay Cause** เมื่องานช้า
- แก้ Defect ที่ถูก assign มา
- ระบุ **Regression From** เมื่อแก้ของเก่าแล้วพังของใหม่

**หมายเหตุ:** สไลด์ระบุ role เป็น `Dev` รวม ไม่ได้แยก FE/BE ในระดับ conditional fields — แต่ในทางปฏิบัติ FE กับ BE มี deliverable และ blocker ต่างกันชัดเจน (FE รอ design + API contract, BE รอ data model + spec)

### KPI ที่ใช้วัด Dev

**Bug Rate** — สัดส่วน Defect ประเภท `Code Bug` ต่อปริมาณงานที่ส่ง

**Estimate Variance** — ประมาณเวลาแม่นแค่ไหน (auto-calculate จาก Initial vs Actual)

### สิ่งที่ปกป้อง Dev ในระบบนี้

ระบบออกแบบมาให้ยุติธรรมกับ Dev มากกว่า tool ทั่วไป:

| กลไก | ปกป้องอย่างไร |
|------|--------------|
| **5 Defect Types** | ปัญหาที่มาจากสเปคไม่ครบเป็น `SA Gap` ไม่ใช่ `Code Bug` — Bug Rate ของ Dev ไม่โดนกระทบ |
| **Delay Cause: Blocked** | ถ้าช้าเพราะรอคนอื่น penalty ย้ายไปที่ Blocker อัตโนมัติ (M05) |
| **Deadline Type: Imposed** | ถ้า deadline ถูกกำหนดมาโดยไม่ได้ตกลงด้วย ระบบแยกออกจาก Committed |

### ความเสี่ยงของตำแหน่งนี้

- **Bug Rate ทำให้กลัวรับงานยาก** — งาน complex มี bug มากกว่าตามธรรมชาติ ถ้าไม่ normalize ตามความยาก Dev จะเลี่ยงงานยาก ควรถ่วงน้ำหนักด้วย complexity หรือ story point
- **การเถียงเรื่อง defect type** — Dev มีแรงจูงใจที่จะ classify bug เป็น SA Gap ต้องมี PM หรือ tech lead ตัดสิน

---

## 4. Tester / QA

### หน้าที่ในระบบ

ตรวจว่าของที่ส่งมาตรงกับ Requirement และไม่พังของเดิม

**งานที่ทำในระบบ:**
- รับ Task ทดสอบ พร้อมกรอก **Pass/Fail Count**
- สร้าง Defect พร้อม **Evidence form บังคับ 3 ส่วน:**
  1. **อ้างอิง** — สเปคบอกว่าควรเป็นอย่างไร
  2. **จริง** — จริงๆ เป็นอย่างไร
  3. **กระทบ** — ส่งผลอะไร
- ระบุ **Found In Stage** — เจอตอนไหน (ใช้วัด cost of defect)
- เลือก **Impacted Roles** (multi-select)
- Verify defect ที่ dev แก้แล้ว → เปลี่ยน status เป็น `Verified`
- ตรวจ **NFR Violation** — ทดสอบว่า metric ที่ SA กำหนดไว้ผ่านจริงไหม

**Conditional Fields ที่ QA ต้องกรอกใน Task:**
| Field | ความหมาย |
|-------|----------|
| **Pass Count** | จำนวน test case ที่ผ่าน |
| **Fail Count** | จำนวนที่ไม่ผ่าน |

### KPI ที่ใช้วัด Tester

**Escape Rate** — สัดส่วน Defect ที่หลุดไปเจอในขั้นถัดไป (`Test Escape`) ต่อ defect ทั้งหมด

`Test Escape` = bug ที่ควรเจอตอน test แต่ไปเจอตอน UAT หรือ production

### จุดที่น่าสังเกต

**Evidence form 3 ส่วนเป็นการออกแบบที่ดีมาก** — บังคับให้ tester อ้างสเปคก่อนแจ้ง bug ตัดปัญหา "bug ที่ไม่ใช่ bug" (tester เข้าใจผิด) ซึ่งกินเวลา dev มหาศาล

**Found In Stage** ทำให้เห็นว่า defect ที่เจอช้าแพงกว่าเจอเร็ว — เป็นข้อมูลที่ใช้ผลักดันให้ทีมลงทุนกับ review ต้นน้ำ

### ความเสี่ยงของตำแหน่งนี้

- **Escape Rate ขึ้นอยู่กับเวลาที่ได้** — ถ้า tester ได้เวลาทดสอบ 1 วันจากที่ควรได้ 3 วัน Escape Rate จะสูงโดยไม่ใช่ความผิด ควรมี field บันทึกว่าเวลาทดสอบพอไหม
- **Pass/Fail Count ไม่บอกคุณภาพของ test** — เขียน test case ง่ายๆ 100 ข้อผ่านหมด ดูดีกว่าเขียน 20 ข้อที่ลึกและเจอปัญหา metric นี้ควรใช้คู่กับ coverage หรือ review จาก lead

---

## 5. PM — Project Manager

### หน้าที่ในระบบ

**ผู้ใช้ระบบหลัก** — PM เป็นคนที่ได้ประโยชน์จากระบบนี้มากที่สุด และมีอำนาจตัดสินหลายจุด

**งานที่ทำในระบบ:**

| งาน | Module |
|-----|--------|
| กำหนด MoSCoW Priority ร่วมกับ SA | M01 |
| แตก Requirement เป็น Task และ assign คน | M02 |
| ดู **PM alert view** ของ Task ที่ถูก block | M02 |
| **Acknowledge Defect** ระดับ Critical/High (DoD บังคับ) | M03 |
| ตัดสิน defect type เมื่อมีข้อพิพาท | M03 |
| จัดการ Sprint ทั้ง Development (fixed) และ Maintenance (rolling) | M04 |
| ดู Completion Rate, Carried Over, Avg Days Late | M04 |
| กรอก **PM Qualitative Score (20% ของ KPI)** | M05 |
| ดู **Cross-project dashboard** และ **Team Workload view** | M06 |
| จัดลำดับ Maintenance Queue (Critical first) | M06 |

### อำนาจและความรับผิดชอบพิเศษ

**PM Acknowledge สำหรับ Critical/High Defect** — เป็น gate ที่บังคับให้ PM รับรู้ปัญหาใหญ่ ไม่ให้ทีมปิด bug สำคัญเงียบๆ

**PM Qualitative Score 20%** — ส่วนที่ระบบวัดไม่ได้: ทัศนคติ การช่วยทีม ความริเริ่ม การสื่อสาร

**Team Workload view (M06)** — ตัวนี้แก้ปัญหาที่ PM หลายคนมอบงานให้คนเดียวกันโดยไม่รู้ว่าอีกโปรเจคก็สั่งไปแล้ว

### ความเสี่ยงของตำแหน่งนี้

- **PM เป็นทั้งผู้ให้คะแนนและผู้มอบงาน** — ถ้า PM มอบงานไม่ดี (deadline Imposed, spec ไม่ครบ) แล้วทีมทำไม่ทัน คนที่ถูกหักคะแนนคือทีม ไม่ใช่ PM ระบบนี้**ไม่มี KPI ของ PM เอง** ซึ่งเป็นช่องว่างที่ควรพิจารณาเพิ่ม
- **20% qualitative อาจกลายเป็นช่องทางลำเอียง** — ควรมี rubric ที่เขียนไว้ชัด ไม่ใช่ให้คะแนนตามความรู้สึก

---

## 6. HR

### หน้าที่ในระบบ

**ผู้ใช้ระบบแบบจำกัดขอบเขต** — HR เข้ามาที่จุดเดียวคือการประเมิน

**งานที่ทำในระบบ:**
- กรอก **HR Assessment score (10% ของ KPI)** — Module 05
- ดู **KPI Summary export per person per Sprint** เพื่อใช้ในการประเมินรอบปี

**สิ่งที่ HR วัด (10%):** โดยทั่วไปคือเรื่องที่ไม่เกี่ยวกับ output ตรงๆ เช่น การเข้างาน การเข้าร่วมกิจกรรม การอบรม พฤติกรรมตามค่านิยมองค์กร

### ข้อสังเกต

น้ำหนัก 10% เหมาะสมแล้ว — HR ไม่ได้ทำงานกับทีมทุกวัน จึงไม่ควรมีอำนาจชี้ขาดมาก แต่ควรมีเสียงเพื่อดูแลมิติที่ PM มองข้าม

---

## โครงสร้าง KPI รวม

```
KPI รายคน = 70% (ระบบคำนวณอัตโนมัติ)
          + 20% (PM Qualitative Score)
          + 10% (HR Assessment)
```

### 70% ที่ระบบคำนวณ แยกตาม Role

| Role | Metric ที่ระบบดึงอัตโนมัติ |
|------|--------------------------|
| **SA** | Gap Rate, Estimate Variance, Completion Rate |
| **UX/UI** | Revision Rate, Completion Rate, Design Gap count |
| **Dev** | Bug Rate, Estimate Variance, Completion Rate, Regression Count |
| **Tester** | Escape Rate, Pass/Fail ratio, Completion Rate |

### กลไกความยุติธรรมที่ฝังอยู่ในระบบ

**1. Delay Cause ย้าย penalty ไป Blocker อัตโนมัติ**

ถ้า Dev A ช้าเพราะรอ Design จาก UX B → penalty ไปที่ B ไม่ใช่ A
นี่เป็นกลไกสำคัญที่ทำให้คน**ไม่กลัวการรายงานว่าถูก block** ซึ่งเป็นข้อมูลที่มีค่าที่สุดสำหรับ PM

**2. Calibration Mode (Sprint 1–2)**

2 sprint แรกเก็บข้อมูลแต่**ไม่นับ KPI** — ให้ทีมเรียนรู้ว่าระบบวัดอะไร ปรับพฤติกรรมได้ก่อนถูกวัดจริง
เป็นการออกแบบที่เข้าใจ change management ดีมาก ป้องกันการต่อต้านระบบใหม่

**3. Deadline Type: Committed vs Imposed**

แยกว่า deadline นี้ทีมตกลงเอง หรือถูกสั่งมา — ถ้า Imposed แล้วทำไม่ทัน ไม่ควรลงโทษเต็มที่

---

## ข้อเสนอแนะสำหรับทีม

### 1. ต้องมีคนกลางตัดสิน Defect Type

`Code Bug` vs `SA Gap` vs `Design Gap` มีผลต่อ KPI ของคนต่างกัน — จะเกิดข้อพิพาทแน่นอน

**ข้อเสนอ:** กำหนดให้ Tech Lead หรือ PM เป็นผู้ตัดสินชี้ขาด และเก็บ log การตัดสินไว้เพื่อสร้าง precedent

### 2. Normalize metric ตามความยากของงาน

Bug Rate ที่ไม่คิดความยากจะทำให้คนเลี่ยงงานยาก
**ข้อเสนอ:** หาร Bug Rate ด้วย Story Point หรือ complexity level

### 3. แยก Revision ที่เป็นความผิด กับที่ไม่ใช่

**ข้อเสนอ:** เพิ่ม field `Revision Reason` ใน UX Task — Requirement Change / Stakeholder Feedback / Design Miss โดยนับ penalty เฉพาะ Design Miss

### 4. เพิ่ม KPI ของ PM

ระบบนี้ยังไม่วัด PM แต่คุณภาพงานของ PM กระทบทุกคน
**ข้อเสนอ:** วัด PM ด้วย % Imposed Deadline, Requirement Change Rate, Blocked Task Resolution Time

### 5. บันทึกเวลาที่ Tester ได้รับ

Escape Rate จะยุติธรรมได้ต้องรู้ว่า tester มีเวลาพอไหม
**ข้อเสนอ:** เพิ่ม field `Test Time Allocated` vs `Test Time Requested`

### 6. เฝ้าระวัง data entry burden

ทุก module บังคับกรอกข้อมูลละเอียด — ถ้ากรอกเกิน 2 นาทีต่อ task ทีมจะเริ่มกรอกส่งเดช
**ข้อเสนอ:** วัดเวลาที่ใช้กรอกฟอร์มจริงใน Sprint 1 (ช่วง Calibration) และตัด field ที่ไม่มีใครใช้ออก

---

## เอกสารที่เกี่ยวข้อง

- `docs/project-modules-overview.md` — สรุป module ทั้งหมด
- `docs/role-checklist.md` — checklist แบ่งงาน/ตรวจงานต่อตำแหน่ง + วิธีใช้ AI-DLC จัดการงาน
