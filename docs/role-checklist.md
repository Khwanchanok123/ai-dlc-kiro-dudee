# Role Checklist — แบ่งงานและตรวจสอบงานต่อตำแหน่ง

> **วัตถุประสงค์:** เอกสารนี้ใช้ 2 อย่าง — (1) **แบ่งงาน** ว่าตำแหน่งไหนรับผิดชอบ stage ไหน (2) **ตรวจงาน** ผ่าน checklist ที่ต้องผ่านก่อนปิดงาน
> **กลไกจัดการ:** ใช้ AI-DLC stage + approval gate เป็นตัวคุมจังหวะงาน
> **เอกสารที่เกี่ยวข้อง:** `project-modules-overview.md`, `team-roles-responsibilities.md`

---

## ⚠️ ข้อควรทราบก่อนใช้งาน

AI-DLC **ยังรันไม่ได้ในเครื่องนี้** เพราะขาด `bun` ซึ่งเป็น prerequisite ของทุกเครื่องมือในเฟรมเวิร์ก

```bash
# ติดตั้งก่อนใช้คำสั่ง /aidlc ใดๆ
curl -fsSL https://bun.sh/install | bash

# ตรวจสอบว่าติดตั้งสำเร็จ
bun --version

# ตรวจสอบว่า AI-DLC พร้อมใช้งาน
/aidlc --doctor
```

> **หมายเหตุสำคัญ:** `bun` ต้องอยู่ใน PATH ของ non-interactive shell ด้วย — ให้เพิ่ม PATH ใน `~/.zshenv` (ไม่ใช่ `~/.zshrc`) เพราะ hook ของ AI-DLC spawn shell แบบ non-interactive

ระหว่างที่ยังไม่ได้ติดตั้ง ใช้ checklist ในเอกสารนี้แบบ manual ได้เลย

---

## ส่วนที่ 1 — แผนที่ตำแหน่ง → AI-DLC Stage

AI-DLC มี 33 stages แบ่งเป็น 4 phase แต่ละ stage มี **lead agent** (คนรับผิดชอบหลัก) และ **support agents** (คนร่วมให้ความเห็น) ตารางนี้แปลง agent ของเฟรมเวิร์กเป็นตำแหน่งจริงในทีม

### การจับคู่ Agent ↔ ตำแหน่งในทีม

| AI-DLC Agent | ตำแหน่งในทีม |
|--------------|-------------|
| `product-agent` | **PM / BA** (เจ้าของ requirement) |
| `architect-agent` | **SA** (ออกแบบระบบ, domain model, contract) |
| `design-agent` | **UX/UI** |
| `developer-agent` | **Dev** (FE + BE) |
| `quality-agent` | **Tester / QA** |
| `delivery-agent` | **PM** (วางแผน sprint, ลำดับงาน) |
| `devsecops-agent` | **Dev BE** (ดูแลความปลอดภัย, secrets) |
| `pipeline-deploy-agent` | **Dev BE / DevOps** |
| `aws-platform-agent` | **Dev BE / DevOps** (infra) |
| `operations-agent` | **DevOps** (observability, incident) |
| `compliance-agent` | **SA / PM** (ถ้าไม่มีตำแหน่งนี้แยก) |

### Stage ที่แต่ละตำแหน่งเป็นเจ้าภาพ (Lead)

| ตำแหน่ง | Stage ที่เป็นเจ้าภาพ | Phase |
|---------|---------------------|-------|
| **PM / BA** | 1.1 Intent Capture, 1.2 Market Research, 1.4 Scope Definition, 2.3 Requirements Analysis, 2.4 User Stories | Ideation, Inception |
| **PM (delivery)** | 1.5 Team Formation, 1.7 Approval & Handoff, 2.9 Delivery Planning | Ideation, Inception |
| **SA** | 1.3 Feasibility, 2.6 Domain Design, 2.7 Units Generation, 2.8 Contract Design, 3.1 Functional Design, 3.2 NFR Requirements, 3.3 NFR Design | Ideation → Construction |
| **UX/UI** | 1.6 Rough Mockups, 2.5 Refined Mockups | Ideation, Inception |
| **Dev** | 2.1 Reverse Engineering, 3.5 Code Generation | Inception, Construction |
| **Tester / QA** | 3.6 Build and Test, 4.6 Performance Validation | Construction, Operation |
| **Dev BE / DevOps** | 2.2 Practices Discovery, 3.4 Infrastructure Design, 3.7 CI Pipeline, 4.1 Deployment Pipeline, 4.2 Environment Provisioning, 4.3 Deployment Execution | Inception → Operation |
| **DevOps** | 4.4 Observability Setup, 4.5 Incident Response, 4.7 Feedback & Optimization | Operation |

### Stage ที่ทำงานร่วมกันหลายตำแหน่ง (Mob / Ensemble)

| Stage | เจ้าภาพ | ร่วมด้วย | รูปแบบ |
|-------|--------|---------|--------|
| **2.4 User Stories** | PM/BA | UX/UI, Dev, Tester | `mob` — ทุกคนอยู่พร้อมกัน |
| **2.2 Practices Discovery** | DevOps | Tester, Dev, DevSecOps | `subagent` — แยกกันคิดแล้วรวม |
| **1.3 Feasibility** | SA | DevOps, Compliance | `inline` |
| **3.2 NFR Requirements** | SA | DevSecOps, Compliance, Tester | `inline` |
| **2.1 Reverse Engineering** | Dev | SA | `pipeline` — ต่อกันเป็นทอด |

> **2.4 User Stories เป็น stage ที่สำคัญที่สุดสำหรับทีมนี้** — เป็นจุดเดียวที่ PM, UX, Dev, Tester ต้องอยู่พร้อมกัน ถ้าข้ามหรือทำคนเดียว จะเกิด `SA Gap` และ `Design Gap` ตามมาทีหลัง (ตาม Module 03)

---

## ส่วนที่ 2 — Checklist ตรวจงานต่อตำแหน่ง

แต่ละ checklist คือเงื่อนไขที่ต้องผ่านก่อนกด **Approve** ที่ approval gate ของ stage นั้น

### 🔵 SA — System Analyst

#### ก่อนเริ่มงาน
- [ ] อ่าน Requirement ต้นทางครบ และเข้าใจ business goal
- [ ] รู้ว่า Requirement นี้อยู่ใน Module ไหน Sprint ไหน
- [ ] ยืนยัน MoSCoW Priority กับ PM แล้ว

#### ตอนทำงาน
- [ ] แยก Req Category ชัดเจน (Functional / Non-Functional)
- [ ] **NFR ทุกตัวมีตัวเลขวัดได้** — ไม่มีคำว่า "เร็ว" "ง่าย" "เสถียร" ลอยๆ
- [ ] ระบุ metric แบบครบ 3 ส่วน: ชื่อ metric, ค่าเป้าหมาย, หน่วย
- [ ] เขียน Acceptance Criteria แบบ Given/When/Then
- [ ] ระบุ edge case และ error case ที่คิดออก
- [ ] ใส่ Initial Estimate ระดับ Requirement
- [ ] ระบุ Deliverable ที่จะส่งมอบ (spec doc / flow / data dictionary)

#### ก่อนปิดงาน (Definition of Done)
- [ ] Requirement ทุกข้อ **testable** — Tester อ่านแล้วเขียน test case ได้ทันที
- [ ] ไม่มี requirement ที่ขัดแย้งกันเอง
- [ ] ทุก requirement trace ย้อนไปหา business intent ได้
- [ ] มีผู้ **Approval** เรียบร้อย (field บังคับใน Module 02)
- [ ] Dev อ่านแล้วยืนยันว่าเข้าใจตรงกัน — **ทำก่อนปิด ไม่ใช่หลัง**
- [ ] UX อ่านแล้วยืนยันว่าออกแบบได้

#### สัญญาณเตือนว่างานยังไม่พร้อม
- ⚠️ Dev ถามคำถามเดิมซ้ำ = สเปคไม่ชัด
- ⚠️ มีคำว่า "ตามที่เคยคุยกัน" ในเอกสาร = ไม่มีหลักฐาน
- ⚠️ NFR ไม่มีตัวเลข = จะเถียงกันตอน UAT แน่นอน

**KPI ที่ถูกวัด:** Gap Rate (จำนวน `SA Gap` ต่อ requirement ที่เขียน)

---

### 🎨 UX/UI Designer

#### ก่อนเริ่มงาน
- [ ] อ่าน Requirement และ Acceptance Criteria ครบ
- [ ] ถามคำถามที่ยังไม่ชัดกับ SA/PM **ก่อน** ลงมือออกแบบ
- [ ] รู้ว่า user คือใคร ใช้ในสถานการณ์ไหน
- [ ] ตรวจว่ามี design system / component library เดิมให้ใช้ไหม

#### ตอนทำงาน
- [ ] ออกแบบ happy path ครบก่อน
- [ ] ออกแบบ **state ที่มักถูกลืม**:
  - [ ] Empty state (ยังไม่มีข้อมูล)
  - [ ] Loading state
  - [ ] Error state
  - [ ] Partial data state
  - [ ] Permission denied state
- [ ] ตรวจ accessibility: contrast ratio, focus order, keyboard navigation, label ของ form
- [ ] ออกแบบ responsive ครบทุก breakpoint ที่ตกลงไว้
- [ ] ใส่ Figma link ใน Task
- [ ] บันทึก **Revision count** พร้อม**เหตุผล**ที่แก้

#### ก่อนปิดงาน (Definition of Done)
- [ ] Dev FE อ่านแล้วยืนยันว่า implement ได้ — ไม่มี component ที่ทำไม่ได้จริง
- [ ] มี spacing / typography / color spec ที่ Dev เอาไปใช้ได้เลย
- [ ] ทุกหน้าที่ออกแบบ trace ไปหา requirement ได้
- [ ] ระบุ interaction behavior ชัดเจน (hover, click, transition)
- [ ] Tester อ่านแล้วรู้ว่าจะทดสอบอะไร

#### สัญญาณเตือน
- ⚠️ Revision เกิน 3 รอบด้วยเหตุผล "Design Miss" = เข้าใจโจทย์ผิดตั้งแต่แรก
- ⚠️ Dev ถามว่า "state นี้หน้าตายังไง" = ออกแบบไม่ครบ → จะกลายเป็น `Design Gap`

**KPI ที่ถูกวัด:** Revision Rate, Design Gap count

> 💡 **ข้อเสนอต่อทีม:** แยก Revision เป็น `Requirement Change` (ไม่นับ penalty) กับ `Design Miss` (นับ) — ไม่งั้นนักออกแบบจะไม่กล้า iterate ให้ดีขึ้น

---

### 💻 Dev FE — Frontend Developer

#### ก่อนเริ่มงาน
- [ ] มี Design ที่ **approve แล้ว** (ไม่ใช่ draft)
- [ ] มี **API contract** ที่ตกลงกับ Dev BE แล้ว — request/response schema ชัดเจน
- [ ] เข้าใจ Acceptance Criteria ทุกข้อ
- [ ] ระบุ Work Pattern (Sequential / Parallel / Independent)
- [ ] ถ้ารอของจากใคร → ใส่ **Blocked By** ทันที ไม่รอให้ช้าก่อน

#### ตอนทำงาน
- [ ] ทำตาม design ที่ approve — ถ้าต่างจาก design ต้องคุยก่อน ไม่แก้เอง
- [ ] Implement ทุก state ที่ design ระบุ (empty / loading / error)
- [ ] Handle error จาก API ทุกกรณี — ไม่มี silent failure
- [ ] Validate input ฝั่ง client (แต่ไม่ใช่แทน server validation)
- [ ] ไม่ hardcode ค่า config / API key / URL
- [ ] Accessibility: semantic HTML, aria-label, keyboard support
- [ ] เขียน test: happy path + error case อย่างน้อย 2 กรณี
- [ ] ถ้างานช้า → กรอก **Days Late + Delay Cause** ตามจริง

#### ก่อนปิดงาน (Definition of Done)
- [ ] Build ผ่าน ไม่มี error / warning ที่สำคัญ
- [ ] Lint + formatter ผ่าน
- [ ] Test ที่เขียนรันผ่าน และ suite เดิมยังเขียว
- [ ] ตรวจกับ design จริงว่าตรง (ไม่ใช่ "ประมาณนั้น")
- [ ] ทดสอบด้วยมือทุก state ที่ design มี
- [ ] ทดสอบ responsive ทุก breakpoint
- [ ] ไม่มี console.log / debug code ค้าง
- [ ] Update Estimate Actual ในระบบ

**KPI ที่ถูกวัด:** Bug Rate, Estimate Variance, Regression Count

---

### ⚙️ Dev BE — Backend Developer

#### ก่อนเริ่มงาน
- [ ] มี Requirement + NFR metric ที่ต้องทำให้ผ่าน
- [ ] มี data model / schema ที่ SA ออกแบบ (stage 2.6 Domain Design)
- [ ] มี **API contract** ที่ตกลงกับ Dev FE แล้ว
- [ ] รู้ว่า NFR ตัวไหนบังคับใช้กับ endpoint นี้ (response time, throughput)
- [ ] ระบุ Work Pattern + Blocked By

#### ตอนทำงาน
- [ ] Implement ตาม contract ที่ตกลง — ถ้าต้องเปลี่ยนต้องแจ้ง FE ก่อน
- [ ] **Validate input ทุกจุดที่รับข้อมูลจากภายนอก** (ไม่ trust client)
- [ ] ใช้ parameterized query — ไม่ต่อ SQL string
- [ ] Error handling ครบทุก integration boundary (DB, external API, file I/O)
- [ ] แยก recoverable error (retry/fallback) กับ fatal error (fail fast)
- [ ] **ไม่ hardcode credentials / API key / secret** — ใช้ env var หรือ secrets manager
- [ ] ตรวจ authorization ทุก endpoint — ไม่มี endpoint ที่เปิดโดยไม่ได้ตั้งใจ
- [ ] เขียน test: happy path + error case อย่างน้อย 2 กรณี
- [ ] วัดว่า NFR metric ผ่านจริง (ไม่ใช่เดา)

#### ก่อนปิดงาน (Definition of Done)
- [ ] Build + migration รันผ่าน
- [ ] Lint ผ่าน
- [ ] Test ผ่าน และ suite เดิมยังเขียว
- [ ] **NFR metric วัดแล้วผ่านจริง** — มีตัวเลขยืนยัน ไม่ใช่ความรู้สึก
- [ ] API ตรงกับ contract ที่ FE ใช้ — ทดสอบเรียกจริงแล้ว
- [ ] ไม่มี secret ใน code หรือ commit history
- [ ] Log พอสำหรับ debug production แต่ไม่ log ข้อมูลอ่อนไหว
- [ ] Update Estimate Actual

#### สัญญาณเตือน
- ⚠️ FE บอกว่า API ไม่ตรงกับที่ตกลง = contract หลุด จะกลายเป็น defect
- ⚠️ ไม่ได้วัด NFR = จะกลายเป็น `NFR Violation` ตอน performance test

**KPI ที่ถูกวัด:** Bug Rate, Estimate Variance, Regression Count, NFR Violation

---

### 🧪 Tester / QA

#### ก่อนเริ่มงาน
- [ ] Requirement + Acceptance Criteria **approve แล้ว**
- [ ] เข้าใจ NFR metric ที่ต้องทดสอบ
- [ ] รู้ว่ามีเวลาทดสอบเท่าไหร่ — ถ้าไม่พอ **แจ้ง PM ทันที** ไม่ใช่รับแล้วไปช้าเอง
- [ ] มี test environment พร้อม และมีข้อมูลทดสอบ

#### ตอนทำงาน
- [ ] เขียน test case จาก Acceptance Criteria ทุกข้อ — ไม่ข้าม
- [ ] ครอบ negative case และ boundary value ไม่ใช่แค่ happy path
- [ ] ทดสอบ state ที่ UX ออกแบบไว้ครบ (empty / error / loading)
- [ ] **ทดสอบ NFR ตามตัวเลขที่ SA กำหนด** — ไม่ใช่แค่ functional
- [ ] ทดสอบ regression ของฟีเจอร์ที่เกี่ยวข้อง
- [ ] บันทึก Pass / Fail Count ตามจริง

#### เมื่อเจอ Defect — บังคับครบทุกช่อง
- [ ] เลือก **Defect Type** ถูกต้อง: `Code Bug` / `SA Gap` / `Design Gap` / `Test Escape` / `NFR Violation`
- [ ] **Evidence 3 ส่วน:**
  - [ ] **อ้างอิง** — สเปค/design ข้อไหนบอกว่าควรเป็นอย่างไร (ต้องอ้างได้จริง)
  - [ ] **จริง** — จริงๆ เป็นอย่างไร พร้อม screenshot / log / step ที่ทำซ้ำได้
  - [ ] **กระทบ** — ส่งผลอะไรต่อ user หรือระบบ
- [ ] ระบุ **Found In Stage** (ใช้วัด cost of defect)
- [ ] เลือก **Impacted Roles**
- [ ] ถ้าเป็นของเก่าที่พังใหม่ → ใส่ **Regression From** (ไม่ reopen ตัวเดิม)
- [ ] ถ้า Critical/High → แจ้ง PM ให้ **Acknowledge**

#### ก่อนปิดงาน (Definition of Done)
- [ ] Test case ครอบ Acceptance Criteria 100%
- [ ] Defect ทุกตัวมี evidence ครบ 3 ส่วน — ไม่มีตัวไหนขาด
- [ ] Defect ที่ dev แก้แล้วถูก **Verify** จริง ไม่ใช่เชื่อว่าแก้แล้ว
- [ ] NFR ทุกตัวมีผลวัดเป็นตัวเลข
- [ ] สรุปว่าฟีเจอร์นี้ปล่อยได้หรือไม่ พร้อมเหตุผล

#### สัญญาณเตือน
- ⚠️ แจ้ง bug โดยไม่มี "อ้างอิง" = อาจเป็นความเข้าใจผิด กิน dev time ฟรี
- ⚠️ เวลาทดสอบถูกบีบ = Escape Rate จะสูง **ต้องบันทึกไว้เป็นหลักฐาน**

**KPI ที่ถูกวัด:** Escape Rate, Pass/Fail ratio

> 💡 **ข้อเสนอต่อทีม:** เพิ่ม field `Test Time Allocated` vs `Test Time Requested` — ไม่งั้น Escape Rate ไม่ยุติธรรมกับ Tester ที่ถูกบีบเวลา

---

### 📋 PM — Project Manager

#### ก่อนเริ่ม Sprint
- [ ] Requirement ที่จะเข้า sprint **approve แล้วทุกตัว**
- [ ] MoSCoW Priority ตกลงกับ SA และ stakeholder แล้ว
- [ ] ตรวจ **Team Workload view ข้ามโปรเจค** — ไม่มอบงานเกินกำลังคน
- [ ] Deadline ทุกตัวระบุ **Committed หรือ Imposed** ตามจริง
- [ ] Task ทุกตัวมีเจ้าภาพชัดเจน
- [ ] Dependency ระหว่าง task ระบุครบ (Blocked By)
- [ ] Tester ได้เวลาทดสอบพอ — ยืนยันกับ Tester แล้ว

#### ระหว่าง Sprint
- [ ] ดู **PM alert view** ของ task ที่ถูก block ทุกวัน และแก้ให้
- [ ] **Acknowledge Defect ระดับ Critical/High** ภายในเวลาที่ตกลง
- [ ] ตัดสิน defect type เมื่อมีข้อพิพาท — และ**บันทึกเหตุผลไว้** เพื่อใช้เป็นบรรทัดฐานครั้งถัดไป
- [ ] ตรวจว่า Delay Cause ที่ทีมกรอกสมเหตุสมผล
- [ ] ติดตาม Requirement Change ที่เกิดขึ้น และประเมินผลกระทบ

#### ปิด Sprint
- [ ] Completion Rate คำนวณครบ
- [ ] Carried Over Tasks มีเหตุผลอธิบายได้ทุกตัว
- [ ] Defect breakdown per type ตรวจแล้ว — ดูว่าปัญหากระจุกที่ stage ไหน
- [ ] Delay Cause distribution ตรวจแล้ว — ถ้า `Blocked` เยอะ คือปัญหาการวางแผน ไม่ใช่ปัญหาคน
- [ ] กรอก **PM Qualitative Score (20%)** ตาม rubric ที่เขียนไว้ ไม่ใช่ความรู้สึก
- [ ] Export KPI Summary ต่อคน

#### สิ่งที่ PM ต้องระวังกับตัวเอง
- ⚠️ **% Imposed Deadline สูง** = กำลังโยนความกดดันให้ทีมโดยไม่ตกลง
- ⚠️ **Requirement Change Rate สูง** = ปัญหาต้นน้ำที่ PM ต้องแก้เอง ไม่ใช่โทษ SA
- ⚠️ **Blocked Task ค้างนาน** = PM ไม่ได้ทำหน้าที่ปลด blocker

> 💡 **ข้อเสนอต่อทีม:** ระบบยังไม่มี KPI ของ PM แต่คุณภาพงาน PM กระทบทุกคน ควรวัดด้วย 3 ตัวข้างบน

---

## ส่วนที่ 3 — Cross-Role Handoff Checklist

จุดที่งานเปลี่ยนมือคือจุดที่ปัญหาเกิดมากที่สุด — checklist นี้ป้องกัน defect ที่ต้นทาง

### SA → UX/UI
- [ ] Requirement approve แล้ว
- [ ] Acceptance Criteria ครบทุกข้อ
- [ ] UX ถามคำถามและได้คำตอบครบ **ก่อน** เริ่มออกแบบ
- [ ] ตกลงกันว่า state ไหนต้องออกแบบ

### SA → Dev BE
- [ ] Data model / schema approve แล้ว
- [ ] NFR metric ระบุเป็นตัวเลข
- [ ] Business rule ทุกข้อเขียนไว้ ไม่ใช่บอกปากเปล่า

### UX/UI → Dev FE
- [ ] Design approve แล้ว (ไม่ใช่ draft)
- [ ] มี spec: spacing, typography, color, interaction
- [ ] ครบทุก state (empty / loading / error / partial)
- [ ] Dev FE ยืนยันว่า implement ได้จริง

### Dev BE ↔ Dev FE (จุดที่พังบ่อยที่สุด)
- [ ] **API contract ตกลงเป็นลายลักษณ์อักษร** ก่อนทั้งสองฝ่ายเริ่มเขียน
- [ ] Schema ของ request/response ชัดเจนทุก field
- [ ] Error response format ตกลงแล้ว
- [ ] มี mock/stub ให้ FE ทำงานคู่ขนานได้ (ไม่ต้องรอ BE เสร็จ)
- [ ] ทดสอบเรียก API จริงร่วมกันก่อนส่ง Tester

### Dev → Tester
- [ ] Build deploy ขึ้น test environment แล้ว
- [ ] Test data พร้อม
- [ ] Dev ระบุว่าแก้อะไรไปบ้าง (สำหรับ regression scope)
- [ ] Unit test ของ dev ผ่านแล้ว — ไม่โยนของพังให้ Tester
- [ ] Tester ยืนยันว่าเวลาที่ได้พอ

### Tester → PM (ปิดงาน)
- [ ] ผลทดสอบสรุปครบ พร้อมตัวเลข
- [ ] Defect ทุกตัวมี evidence ครบ
- [ ] NFR วัดแล้วผ่าน
- [ ] มีข้อสรุปชัดว่าปล่อยได้/ไม่ได้ พร้อมเหตุผล

---

## ส่วนที่ 4 — วิธีใช้ AI-DLC จัดการงานจริง

### คำสั่งพื้นฐาน

```bash
# ตรวจว่าระบบพร้อม (ทำก่อนอย่างอื่นเสมอ)
/aidlc --doctor

# ดูว่างานอยู่ที่ไหนแล้ว
/aidlc --status

# เริ่มงานใหม่ — บอกว่าจะทำอะไร ระบบจะเลือก stage ให้เอง
/aidlc "สร้าง Requirement Management module ตาม docs/project-modules-overview.md"

# ขอแผนที่พอดีกับงาน ก่อนเริ่มจริง (ระบบเสนอ ทีมอนุมัติ)
/aidlc compose "สร้าง Requirement Management module"
```

### สั่งงานเฉพาะตำแหน่ง

แต่ละ stage รันแยกได้ ใช้ตอนที่ต้องการให้ตำแหน่งใดตำแหน่งหนึ่งทำงาน:

| ตำแหน่ง | คำสั่ง | ได้อะไร |
|---------|-------|---------|
| **PM/BA** | `/aidlc-requirements-analysis` | Requirement ที่ testable |
| **PM/BA + ทีม** | `/aidlc-user-stories` | User story พร้อม Given/When/Then |
| **SA** | `/aidlc-domain-design` | Domain model, entity, relation |
| **SA** | `/aidlc-contract-design` | API contract |
| **SA** | `/aidlc-nfr-requirements` | NFR ที่มีตัวเลขวัดได้ |
| **UX/UI** | `/aidlc-rough-mockups` | Mockup ระดับร่าง |
| **UX/UI** | `/aidlc-refined-mockups` | Mockup พร้อมส่ง Dev |
| **Dev** | `/aidlc-code-generation` | Code |
| **Tester** | `/aidlc-build-and-test` | Test + ผลรัน |
| **PM** | `/aidlc-delivery-planning` | ลำดับงานและ Bolt |
| **DevOps** | `/aidlc-ci-pipeline` | CI config |

> **ข้อควรรู้:** คำสั่ง `/aidlc-<stage>` รัน stage เดียวแบบแยกตัว **ไม่ขยับ workflow หลัก** เหมาะกับงานที่ต้องการผลลัพธ์เฉพาะจุด ถ้าต้องการให้งานเดินตามลำดับจริงให้ใช้ `/aidlc` เปล่า

### จังหวะการทำงานที่แนะนำสำหรับทีมนี้

```
1. PM     → /aidlc compose "..."           → ตกลงแผนร่วมกันก่อน
2. PM/BA  → /aidlc-requirements-analysis   → SA ตรวจว่า testable
3. ทีม     → /aidlc-user-stories            → PM+UX+Dev+Tester พร้อมกัน ⭐
4. UX     → /aidlc-refined-mockups          → Dev FE ตรวจว่าทำได้
5. SA     → /aidlc-domain-design            → Dev BE ตรวจว่าเหมาะสม
6. SA     → /aidlc-contract-design          → FE+BE ตกลงร่วมกัน ⭐
7. SA     → /aidlc-nfr-requirements         → Tester ตรวจว่าวัดได้
8. PM     → /aidlc-delivery-planning        → แบ่ง Bolt ตาม workload
9. Dev    → /aidlc-code-generation          → ทำตาม Bolt
10. Tester → /aidlc-build-and-test          → ตรวจครบตาม checklist
```

⭐ = จุดที่ทีมต้องอยู่พร้อมกัน ห้ามทำคนเดียว

### Approval Gate คือจุดตรวจงาน

ทุก stage จบด้วย approval gate — **ใช้ checklist ในส่วนที่ 2 ตรวจก่อนกด Approve**

- กด **Approve** เมื่อ checklist ผ่านครบ
- กด **Request Changes** พร้อมบอกว่าอะไรไม่ผ่าน — ระบบจะวนกลับมาแก้
- อย่ากด Approve เพราะอยากให้งานเดิน — gate มีไว้เพื่อกันปัญหาไหลลงน้ำ

### บันทึกบทเรียนให้ระบบจำ

ทุก stage จะถามว่ามีอะไรจะเพิ่มไหม (learnings ritual) — ถ้าทีมเจอปัญหาซ้ำ ให้บันทึกไว้ ระบบจะจำเป็นกฎถาวรใน `aidlc/spaces/default/memory/team.md` และเตือนครั้งถัดไป

**ตัวอย่างที่ควรบันทึก:**
- "ALWAYS ตกลง API contract ก่อน FE และ BE เริ่มเขียน"
- "ALWAYS ให้ Tester ยืนยันเวลาทดสอบก่อนปิด sprint plan"
- "NEVER ปิด Requirement ที่ NFR ไม่มีตัวเลข"

---

## ส่วนที่ 5 — สรุปตารางตรวจงานแบบย่อ

ใช้เป็น checklist เร็วตอน daily หรือ review

| ตำแหน่ง | 3 ข้อที่ต้องผ่านก่อนปิดงาน | KPI ที่ถูกวัด |
|---------|---------------------------|--------------|
| **SA** | ① Requirement testable ② NFR มีตัวเลข ③ Dev+UX ยืนยันเข้าใจตรงกัน | Gap Rate |
| **UX/UI** | ① ครบทุก state ② Dev FE ยืนยันทำได้ ③ มี spec ให้ Dev ใช้ได้เลย | Revision Rate |
| **Dev FE** | ① ตรง design จริง ② Test ผ่าน+suite เขียว ③ API ตรง contract | Bug Rate |
| **Dev BE** | ① NFR วัดผ่านจริง ② ไม่มี secret ใน code ③ Error handling ครบ boundary | Bug Rate, NFR Violation |
| **Tester** | ① ครอบ AC 100% ② Evidence ครบ 3 ส่วน ③ Verify ของที่แก้แล้วจริง | Escape Rate |
| **PM** | ① Workload ไม่เกินกำลัง ② Blocked task ปลดแล้ว ③ Deadline ระบุ Committed/Imposed ตรง | (ยังไม่มี — ควรเพิ่ม) |

---

## เอกสารที่เกี่ยวข้อง

- `docs/project-modules-overview.md` — สรุป 8 modules ของระบบ
- `docs/team-roles-responsibilities.md` — วิเคราะห์หน้าที่และ KPI ของแต่ละตำแหน่งเชิงลึก
- `AGENTS.md` — คู่มือ AI-DLC ของโปรเจกต์นี้
