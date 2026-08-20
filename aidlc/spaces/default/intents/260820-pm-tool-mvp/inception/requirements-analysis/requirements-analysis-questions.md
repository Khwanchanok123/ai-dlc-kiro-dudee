# Requirements Analysis — คำถามเพื่อความชัดเจน

**Stage:** requirements-analysis
**Depth:** Minimal (express scope)
**สร้างเมื่อ:** 2026-08-20

บริบทที่มีอยู่แล้ว (ไม่ต้องถามซ้ำ):
- 3 modules ที่จะทำ: Requirement Management, Task Management, Defect Tracking
- รายละเอียด feature ของแต่ละ module มีอยู่ใน `docs/project-modules-overview.md`
- ตำแหน่งในทีม: SA, UX/UI, Dev FE, Dev BE, Tester, PM (จาก `docs/team-roles-responsibilities.md`)
- ทีม 3-4 คนต่อ module
- เป้าหมาย: MVP

---

## Q1. Tech stack ที่จะใช้

ยังไม่มีการตัดสินใจเรื่องเทคโนโลยีในโปรเจกต์นี้ (workspace ตรวจพบว่าเป็น greenfield ไม่มี code เดิม) การเลือก stack กระทบการแบ่ง unit of work โดยตรง เพราะกำหนดว่างาน FE กับ BE แยกกันได้แค่ไหน

- A. **Next.js full-stack + PostgreSQL** — FE/BE อยู่ repo เดียว แชร์ type ได้ ลด contract mismatch เหมาะกับทีมเล็กและงานเร็ว
- B. **React (FE) + NestJS (BE) แยก + PostgreSQL** — แยกชัด ทีม FE/BE ทำงานขนานกันได้เต็มที่ แต่ต้องตกลง API contract ก่อน
- C. **React (FE) + Express/Hono (BE) แยก + PostgreSQL** — เบากว่า NestJS ตั้งต้นเร็วกว่า แต่โครงสร้างต้องกำหนดเอง
- D. **ทีมมี stack ที่ใช้อยู่แล้ว** — ระบุใน Other
- E. **ให้เลือกให้ตามความเหมาะสม** — จะเลือก A เพราะเร็วที่สุดสำหรับ MVP และทีมขนาดนี้
- X. Other (please specify)

[Answer]: B

---

## Q2. ขอบเขต MVP ของแต่ละ module

`docs/project-modules-overview.md` ระบุ feature ไว้ 7-8 ข้อต่อ module (รวม ~22 ข้อ) ซึ่งมากเกินกว่าจะทำครบใน MVP รอบแรก ต้องตัดสินใจว่าตัดที่ไหน

- A. **Core CRUD + traceability เท่านั้น** — สร้าง/แก้/ลบ/ดู requirement, task, defect ได้ และ link กันได้ (req→task→defect) ตัด KPI, dual estimate, evidence enforcement, status workflow ออกไปรอบหน้า
- B. **Core CRUD + traceability + status workflow** — เพิ่มสถานะของทั้ง 3 entity เพื่อ track progress ได้
- C. **Core CRUD + traceability + status workflow + evidence enforcement** — เพิ่มการบังคับกรอก evidence 3 ส่วนของ defect และ NFR metric validation ซึ่งเป็นหัวใจของระบบตามที่ออกแบบไว้
- D. **ครบทุก feature ตามเอกสาร** — รวม KPI dashboard, dual estimate auto-rollup, multi-project
- E. **ให้เลือกให้ตามความเหมาะสม** — จะเลือก C เพราะ evidence enforcement และ NFR validation คือสิ่งที่ทำให้ระบบนี้ต่างจาก PM tool ทั่วไป ถ้าตัดออกจะเหลือแค่ Trello
- X. Other (please specify)

[Answer]: A

---

## Q3. องค์ประกอบทีมในแต่ละ module

ทีม 3-4 คนต่อ module × 3 modules = 9-12 คน ต้องรู้ว่าแต่ละทีมมีตำแหน่งอะไรบ้าง เพื่อแบ่ง unit of work ให้ตรงกับคนที่มีอยู่จริง

- A. **แต่ละทีมมี: Dev FE 1, Dev BE 1, Tester 1** (3 คน) — SA และ PM เป็นส่วนกลางดูแลทั้ง 3 modules
- B. **แต่ละทีมมี: SA 1, Dev FE 1, Dev BE 1, Tester 1** (4 คน) — SA ประจำแต่ละทีม PM เป็นส่วนกลาง
- C. **แต่ละทีมมี: Dev FE 1, Dev BE 2, Tester 1** (4 คน) — เน้นกำลัง BE เพราะ business logic หนัก
- D. **แต่ละทีมมี: Dev Fullstack 2, Tester 1, SA 1** (4 คน) — ไม่แยก FE/BE
- E. **ให้เลือกให้ตามความเหมาะสม** — จะเลือก B เพราะ SA ประจำทีมลด SA Gap ได้มากที่สุด และตรงกับ 4 คนต่อทีม
- X. Other (please specify)

[Answer]: D

---

## Q4. Authentication และ authorization ใน MVP

ระบบนี้เก็บ KPI รายคนและมีการ Acknowledge ของ PM ซึ่งต้องรู้ว่าใครเป็นใคร แต่ auth ที่ครบถ้วนกินเวลาไม่น้อย

- A. **ยังไม่ทำ auth ใน MVP** — เลือก user จาก dropdown ไปก่อน เน้นให้ core flow เสร็จ (มีความเสี่ยง: ใครก็แก้ข้อมูลของใครได้)
- B. **Login พื้นฐาน + role-based access** — email/password + แยกสิทธิ์ตาม role (SA/Dev/Tester/PM) ป้องกันการแก้ข้อมูลข้ามสิทธิ์
- C. **Login พื้นฐานอย่างเดียว ยังไม่แยก role** — รู้ว่าใคร login แต่ทุกคนทำได้ทุกอย่าง
- D. **ใช้ SSO/OAuth ขององค์กร** — ระบุ provider ใน Other
- E. **ให้เลือกให้ตามความเหมาะสม** — จะเลือก B เพราะระบบนี้มี KPI รายคนและ PM Acknowledge ถ้าไม่แยกสิทธิ์ ข้อมูล KPI จะเชื่อถือไม่ได้เลย
- X. Other (please specify)

[Answer]: A

---

## Q5. (คำถามติดตาม) ขัดแย้งกันระหว่าง Q1 กับ Q3 — ต้องเลือกอย่างใดอย่างหนึ่ง

คำตอบ Q1 และ Q3 ขัดกันในทางปฏิบัติ:

| | คำตอบที่เลือก | นัยของมัน |
|---|---|---|
| **Q1 = B** | React (FE) + NestJS (BE) **แยก repo/service** | ต้องตกลง API contract ก่อนเริ่ม เพราะคนละคนทำคนละฝั่ง — เหตุผลหลักที่เลือก B คือ "ทีม FE/BE ทำงานขนานกันได้เต็มที่" |
| **Q3 = D** | Dev **Fullstack** 2 คน (ไม่แยก FE/BE) | คนเดียวทำทั้ง FE และ BE ของ feature หนึ่ง — ไม่มีการทำงานขนานแบบ FE/BE ให้แยก |

ทำไมจึงขัดกัน: การแยก FE/BE เป็นสอง service มีต้นทุน — ต้องเขียน API contract, จัดการ CORS, deploy 2 ตัว, sync type ระหว่างสองฝั่งด้วยมือ ต้นทุนนี้**คุ้ม**เมื่อมีทีม FE และทีม BE แยกกันทำขนาน แต่เมื่อ dev เป็น fullstack คนเดียวทำทั้งสองฝั่ง ต้นทุนนี้กลายเป็นภาระที่ไม่ได้ประโยชน์อะไรตอบแทน และกินเวลาที่คุณบอกว่าเหลือน้อย

เลือกทางใดทางหนึ่ง:

- A. **เปลี่ยน Q1 เป็น Next.js full-stack** (Q3 = D คงเดิม) — fullstack dev ทำงานใน repo เดียว แชร์ TypeScript type ระหว่าง FE/BE ได้ตรงๆ ไม่ต้องเขียน contract แยก เร็วที่สุดสำหรับ MVP และเข้ากับทีม fullstack พอดี
- B. **คง Q1 = B (React + NestJS แยก) และเปลี่ยน Q3 เป็นแยก FE/BE** — ทีมละ Dev FE 1, Dev BE 1, Tester 1, SA 1 ได้ประโยชน์จากการแยก service จริง แต่ต้องลงทุนเวลาตกลง API contract ก่อน
- C. **คงทั้งสองอย่างไว้ตามที่ตอบ** — React + NestJS แยก แต่ fullstack dev ทำทั้งสองฝั่ง ยอมรับต้นทุนการแยกโดยไม่ได้ประโยชน์การทำขนาน (เลือกข้อนี้ได้ถ้าทีมคุ้นกับ NestJS อยู่แล้วและไม่อยากเรียน Next.js)
- X. Other (please specify)

[Answer]: X. Other (please specify) — "เน้น FE ก่อน"

---

## Q6. (คำถามติดตาม) "เน้น FE ก่อน" — ยืนยันการตีความและเลือก stack ปิดท้าย

"เน้น FE ก่อน" เป็นการเลือก**ลำดับการส่งมอบ** ไม่ใช่การเลือก stack ดังนั้นยังต้องปิดเรื่อง stack ให้จบ แต่คำตอบนี้ทำให้ตัวเลือกแคบลงชัดเจน

ผมตีความว่า: อยากเห็นหน้าจอที่กดใช้งานได้ก่อน แล้วค่อยต่อ backend จริงทีหลัง

การเลือกนี้ **ตัดตัวเลือก React + NestJS แยก service ออกไปเอง** เพราะถ้าจะทำ FE ก่อนโดยที่ BE ยังไม่มี ต้องเขียน mock API ขึ้นมาคั่น แล้วรื้อทิ้งตอน BE เสร็จ — เป็นงานที่ทำแล้วทิ้ง กินเวลาที่เหลือน้อยอยู่แล้ว

- A. **Next.js full-stack + PostgreSQL, ส่งมอบ FE ก่อน** — เริ่มจากหน้าจอทั้งหมดที่ใช้ mock data ในไฟล์ TypeScript เดียวกัน แล้วเปลี่ยน mock เป็น server action / API route ต่อ DB จริงทีละจุด ไม่ต้องเขียน mock server แยก ไม่ต้องรื้อทิ้ง type เดิมใช้ต่อได้ทั้งหมด
- B. **React + Vite (FE เท่านั้น) ใน MVP นี้ ยังไม่ทำ BE เลย** — เก็บข้อมูลใน localStorage ก่อน ได้ของที่กดใช้งานได้และ demo ได้ใน 4 ชั่วโมงแน่นอน แล้วค่อยเพิ่ม backend เป็นงานรอบถัดไป (ข้อจำกัด: ข้อมูลอยู่แต่ในเครื่องคนที่เปิด แชร์กันไม่ได้)
- C. **React + NestJS แยก ตามที่ตอบไว้เดิม โดยทำ FE ก่อนแล้ว mock API ไว้** — ยอมเสียเวลาเขียน mock ที่จะรื้อทิ้ง
- X. Other (please specify)

[Answer]: B

---

## หมายเหตุ: คำตอบที่ถูกแทนที่

- **Q1 (เดิม `B` = React + NestJS แยก)** ถูกแทนที่ด้วยคำตอบ Q6 = `B` (React + Vite, FE เท่านั้น, localStorage) — NestJS ไม่อยู่ใน MVP รอบนี้
- **Q4 (`A` = ไม่มี auth)** ยังคงเดิมและสอดคล้องกัน: เมื่อไม่มี backend ในรอบนี้ จึงไม่มี server ให้ authenticate อยู่แล้ว
- **Q3 (`D` = Dev Fullstack 2 คน)** ยังคงเดิม แต่ในรอบนี้ไม่มีงาน backend ให้ทำ ดังนั้น dev ทั้งสองคนจะทำ FE ทั้งคู่ ทักษะ backend จะได้ใช้ในรอบถัดไปเมื่อเปลี่ยน localStorage เป็น API จริง

---

## Consolidated Summary Confirmation

สรุปการตัดสินใจทั้งหมดก่อนสร้างเอกสาร requirements:

- **Tech stack**: React + Vite เท่านั้น (frontend only) ไม่มี backend ในรอบนี้
- **ที่เก็บข้อมูล**: `localStorage` ของ browser — ข้อมูลอยู่แค่ในเครื่องที่เปิด ไม่แชร์ข้ามผู้ใช้
- **ขอบเขต MVP**: Core CRUD + traceability เท่านั้น — สร้าง/อ่าน/แก้/ลบ ของ Requirement, Task, Defect และเชื่อมโยงกันได้ (Requirement → Task → Defect)
- **ตัดออกจากรอบนี้**: KPI dashboard, dual estimate auto-rollup, evidence enforcement 3 ส่วน, NFR metric validation, status workflow, multi-project, sprint management
- **3 modules ที่ทำ**: Requirement Management, Task Management, Defect Tracking
- **องค์ประกอบทีม**: ทีมละ 4 คน — Dev Fullstack 2, Tester 1, SA 1 (รวม 12 คนใน 3 ทีม) โดยรอบนี้ dev ทั้งสองทำ FE
- **Authentication**: ไม่มีในรอบนี้ — เลือกผู้ใช้จาก dropdown แทน
- **ลำดับการส่งมอบ**: FE ก่อน โดยตั้งใจให้ backend เป็นงานรอบถัดไป
- **ความเสี่ยงที่รับรู้และยอมรับแล้ว**: (1) ไม่มี auth = ใครก็แก้ข้อมูลของใครได้ ข้อมูล KPI ในอนาคตจะเชื่อถือไม่ได้จนกว่าจะเพิ่ม auth (2) localStorage = ทีมแชร์ข้อมูลกันไม่ได้ ต้องเขียน data layer ใหม่ตอนขึ้น backend (3) ตัด evidence enforcement และ NFR validation ออก ทำให้รอบนี้ยังไม่ได้คุณค่าที่ทำให้ระบบต่างจาก PM tool ทั่วไป

Does this all look correct before I generate the requirements artifact?

- Looks correct
- Request changes

[Answer]: Looks correct

---
