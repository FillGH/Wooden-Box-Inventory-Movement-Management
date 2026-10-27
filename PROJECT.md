# Project Definition — Wooden Box Inventory & Movement Management

> **Engineering Source of Truth** · Baseline **v0.1 (Draft)** · Lifecycle **DEFINE → DESIGN** · Updated **2026-10-28**
>
> ข้อมูลนี้สรุปเฉพาะที่ตกลงหรือรับทราบจากผู้ใช้จนถึงปัจจุบัน แยก **CONFIRMED** ออกจาก **PROPOSED / TBD** อย่างจงใจ และยังไม่อนุมัติให้ BUILD

## 1. Problem

Cutting & Packing รับลังไม้และส่วนประกอบจาก Supplier หลายราย มีการตรวจนับและจดการรับจริง/ของขาด/ชำรุดบนกระดาษ ข้อมูลสถานะกระจายอยู่ระหว่างผู้รับของ ผู้ประสานข้อมูล และผู้เบิกใช้ ชิ้นส่วนอาจทยอยส่งไม่ครบชุด ขณะเดียวกันการจัดเก็บกระจายหลายพื้นที่ชั่วคราวและมีการย้ายโดยไม่ได้อัปเดตสต๊อก ทำให้ค้นหาของช้าและต้องส่งคนไปตรวจนับใหม่

**Problem statement (CONFIRMED):** ไม่มีแหล่งข้อมูลที่ทีมหน้างานเชื่อถือได้สำหรับตอบว่า **วัสดุลังกระจกรายการใด รับเข้าจริงแล้วเท่าไร เหลือเท่าไร อยู่ที่ไหน และถูกย้าย/ใช้/ชำรุดไปอย่างไร**

ปัญหาความไม่สอดคล้องระหว่างแผนผลิตกับกำหนดส่งเคยเกิดขึ้นจริง แต่ **การเชื่อม PPIC/Purchasing ถูกตัดออกจากขอบเขต V1 อย่างชัดเจน**; ระบบนี้จึงไม่สามารถรับประกันความพร้อมของวัสดุเทียบกับแผนผลิตได้โดยอัตโนมัติ

## 2. Desired Outcome / Success Criteria

**CONFIRMED — outcomes:**
- คนรับของบันทึกข้อมูลที่แหล่งต้นทาง เพื่อลดการส่งต่อผ่านคนกลาง
- หัวหน้างานและคนเบิกใช้ค้นข้อมูลคงเหลือและสถานที่จากแหล่งเดียวได้
- รับข้อมูล partial receipts, movement และ damage ได้โดยไม่ทำยอดคลาดเคลื่อน
- ส่งออกข้อมูลไปใช้ใน Excel ได้

**PROPOSED — วิธีวัดก่อน/หลัง (target ยังไม่กำหนด):**
- เวลาค้นหาลังหรือยืนยันยอดสต๊อก
- ความตรงกันระหว่างยอดระบบกับยอดตรวจนับจริง
- สัดส่วน movement ที่มีการบันทึกตามกระบวนการ
- เวลาและจำนวนครั้งที่ต้องโทรถาม/นับซ้ำ

## 3. Users / Stakeholders

**CONFIRMED in-process group:**
1. ผู้รับลังไม้ (Receiver)
2. Supplier ที่นำสินค้ามาส่ง (การเข้าใช้ระบบโดยตรง: TBD)
3. หัวหน้างานที่ต้องนำข้อมูลไปใช้ (Supervisor)
4. พนักงานขับ Forklift ที่ย้ายหรือเบิกวัสดุ (Forklift Operator)

**Process owner:** Cutting & Packing (ตามขอบเขตงานที่กำหนด); ผู้มีสิทธิอนุมัติและผู้ดูแลเทคนิค: TBD

PPIC และ Purchasing เป็นหน่วยงานใน **บริบทกระบวนการเดิมเท่านั้น** ไม่ใช่ผู้ที่ต้องใช้หรือจัดการข้อมูลในแอป V1

## 4. Constraints

- **CONFIRMED:** มือถือส่วนตัว (BYOD) ใช้ได้; หน้างานไม่มี PC/Tablet ของบริษัทให้ใช้เป็นประจำ
- **CONFIRMED:** อินเทอร์เน็ตอาจไม่ต่อเนื่อง; ห้ามสมมติว่าติดต่อ server ได้ตลอด
- **CONFIRMED:** ผู้ใช้มีทักษะดิจิทัลต่างกัน; หน้าจอภาษาไทย เรียบง่าย และลดจำนวนครั้งที่ต้องพิมพ์
- **CONFIRMED:** ข้อมูลไม่ต้องเปิดเป็นสาธารณะ ต้องควบคุมผู้เข้าถึง
- **CONFIRMED:** ไม่ใช้ QR Code/Barcode Scan
- **CONFIRMED:** Web App (หากเลือกใช้) ต้องอยู่บน Cloudflare
- **SAFETY:** พนักงานต้องไม่กรอกข้อมูลขณะขับรถ Forklift หรือกำลังเคลื่อนย้ายของ; จุดและวิธีใช้งานจริงให้ทบทวนกับหน้างาน
- **TBD:** IT/cloud/BYOD policy, งบประมาณ, ผู้ดูแล, อายุข้อมูล, backup และการทำงานขณะ offline

## 5. Assumptions / Unknowns

- **KNOWN:** หนึ่งลังโดยทั่วไปใช้ Top 1, Bottom 1, Side 2 ชิ้น และ Supplier อาจส่งชิ้นส่วนแยก/หลายรอบ
- **TBD:** SKU หมายถึงรหัสลัง หรือรหัสสินค้ากระจก, และ uniqueness ของการจับคู่ Product/Spec
- **TBD:** สามารถใช้ material เดียวกันข้ามใบส่งของ/พื้นที่/PO ได้หรือไม่
- **TBD:** นิยามการเบิกใช้จริงกับจุดที่ถือว่า stock ถูกใช้หมด
- **TBD:** รับชิ้นส่วนเสียหายอย่างไร, ใครอนุมัติแก้ยอด, และแก้ข้อมูลผิดอย่างไร
- **TBD:** ต้องบันทึก offline จริงหรือไม่ และความเสี่ยง conflict ยอมรับได้เพียงใด
- **TBD:** การส่งออกต้องเป็น .xlsx โดยตรงหรือ CSV ที่เปิด Excel ได้
- ดูคำถามที่จัดลำดับไว้ใน [docs/OPEN_QUESTIONS.md](docs/OPEN_QUESTIONS.md)

## 6. Scope

### In Scope — CONFIRMED
- [x] รับเข้า (Receiving Record) พร้อมรายละเอียดสินค้าและจำนวน Top / Bottom / Side
- [x] รองรับ Supplier ที่ส่งครบชุดและทยอยส่งไม่ครบชุด
- [x] Movement transactions แบบง่าย
- [x] ติดตามตำแหน่งจัดเก็บและจำนวนคงเหลือ
- [x] ข้อมูลชำรุด/ข้อสังเกตตอนรับและระหว่างจัดเก็บ/ใช้
- [x] Export เป็นข้อมูลสำหรับ Excel
- [x] รองรับผู้ใช้หน้างานผ่านมือถือ

### Out of Scope — CONFIRMED
- [x] QR / Barcode scanning
- [x] PPIC production planning, plan import/synchronization
- [x] Purchasing workflow, PO import/synchronization/management
- [x] ERP integration, automated plan-vs-stock readiness and supplier ETA logic
- [x] Price comparison, costing, accounting/payment
- [x] AI/OCR อ่านใบส่งของ (ไม่ได้ร้องขอสำหรับ V1)

**ข้อยกเว้นที่ตั้งใจ:** เก็บ PO **4 ตัวท้าย** เป็น reference field ใน Receiving ไม่ใช่การนำ PO เข้าแอปเพื่อบริหารการจัดซื้อ

## 7. Solution / Design

**CONFIRMED direction:** ระบบบันทึกของจริงใน Cutting & Packing: **Receive → Store → Move → Issue → Track** พร้อมความสามารถติดตามของชำรุดและ Export

**PROPOSED implementation (ยังไม่อนุมัติ):** Mobile-first Web App บน Cloudflare, server-authoritative inventory ledger, role-based access, ตาราง Location และรายการ/สต๊อก โดยออกแบบ interaction ก่อน integration. ห้ามถือว่า Cloudflare Pages/Workers/D1/Access ถูกอนุมัติแล้วเพียงเพราะแพลตฟอร์ม Cloudflare เป็นทิศทางที่กำหนด

**กติกาที่ควรรักษา (PROPOSED):**
- Stock ไม่ใช่ช่องตัวเลขที่แก้ทับได้อย่างอิสระ แต่คำนวณจาก transactions
- Move เปลี่ยนที่เก็บ ไม่เปลี่ยนยอดรวม; Issue ลด usable stock; Damage แยก unusable ออกจาก usable
- รับแยกจำนวน component เพื่อให้ partial delivery ไม่ถูกนับเป็นชุดครบโดยอัตโนมัติ
- ข้อมูลที่มีอยู่แล้วให้ระบบช่วยเติมจากรายการก่อนหน้าได้ หากผู้ใช้เห็นชอบ

### UI/UX interaction baseline
- [ ] สร้าง mobile prototype ด้วยข้อมูลสมมติ
- [ ] ทดสอบ Receive, Move, Issue, Damage, Search, Export รวมทั้งข้อมูลไม่ครบ/กรอกผิด
- [ ] ทดสอบกับคนหน้างานและหัวหน้างานบนมือถือ
- [ ] ผู้ใช้ยืนยัน interaction baseline
- [ ] ลงบันทึก prototype ที่ยอมรับแล้ว

**Accepted baseline:** ยังไม่มี

### Data lifecycle
Data owner, restore, export policy, source of truth, retention, delete/correction, offline queue: **TBD** ก่อนการใช้งานจริง

### Alternatives / Trade-offs (ไม่ใช่คำตัดสิน)
| Option | Benefit | Cost / Risk | Status |
|---|---|---|---|
| กระดาษ/Excel เดิม | เปลี่ยนน้อย | ไม่มี stock truth ที่ทันเวลา | ปัญหาปัจจุบัน |
| Excel เป็นฐานข้อมูลหลัก | คุ้นเคย | เสี่ยงการแก้พร้อมกัน/ย้ายออฟไลน์/traceability | ไม่ใช่ทิศทางหลัก |
| Mobile-first Cloudflare Web App | UX เฉพาะงาน, เข้าถึงผ่านมือถือ | auth, internet, data integrity, maintenance | แนวทางที่ต้องออกแบบ |

## 8. Requirements

### Functional — CONFIRMED
- **F01** บันทึกรับเข้า: Description, SKU, จำนวนแผ่นต่อลัง, ขนาดกระจก, Top/Bottom/Side Qty, PO 4 ตัวท้าย, Invoice Number, วันที่รับ, พื้นที่รับลง, ชื่อผู้รับ, Remark
- **F02** รองรับจำนวนรับจริงไม่ครบส่วนประกอบ และการรับหลายรอบ
- **F03** บันทึก Stock Movement แบบสั้นและตรวจสอบย้อนหลังได้ (ชนิดย่อยและบทบาทผู้บันทึก: TBD)
- **F04** ค้นดูยอดคงเหลือและพื้นที่ปัจจุบัน
- **F05** บันทึก/แยกสถานะชำรุด
- **F06** Export ข้อมูลเพื่อเปิดวิเคราะห์ใน Excel
- **F07** ใช้งานบนมือถือที่เหมาะกับผู้ใช้หน้างาน

### Non-functional — CONFIRMED / TO DESIGN
- **N01** ข้อมูลระบบต้องไม่เปิดเผยต่อผู้ไม่ได้รับอนุญาต
- **N02** รองรับสถานการณ์อินเทอร์เน็ตไม่แน่นอน (พฤติกรรม offline ยัง TBD)
- **N03** ลดการกรอกซ้ำและทำ UI ภาษาไทย เข้าใจง่าย
- **N04** หากทำ Web App ใช้ Cloudflare และต้องผ่านข้อกำหนดบริษัทก่อนใช้งานจริง
- **N05 (PROPOSED)** ป้องกัน movement ซ้ำ ยอดติดลบ และความคลาดเคลื่อนเมื่อแก้หรือบันทึกขณะ offline

ดูรายละเอียดข้อมูลใน [docs/DATA_AND_MOVEMENT_DRAFT.md](docs/DATA_AND_MOVEMENT_DRAFT.md)

## 9. Acceptance Criteria — Draft (NOT TESTED)

- **AC01:** บันทึกจำนวน Top, Bottom, Side ไม่เท่ากันได้ โดยไม่ตีความว่าครบตามจำนวนลังที่สั่ง
- **AC02:** ค้นหารายการแสดงจำนวน usable ที่ถูกต้องตาม transactions และ Location
- **AC03:** Move 1 รายการจาก A ไป B ทำยอดรวมคงเดิม แต่เปลี่ยนยอดตาม Location
- **AC04:** Issue 1 รายการหักจำนวนถูกต้อง; ไม่บันทึกยอดซ้ำหากกดยืนยันซ้ำ
- **AC05:** Damage แยกของใช้ไม่ได้ออกจากยอดใช้ได้
- **AC06:** Export แล้วตรวจสอบข้อมูลใน Excel ได้โดยไม่เสียข้อมูลสำคัญ
- **AC07:** ผู้ใช้งานที่ไม่มีสิทธิ์เข้าถึงข้อมูลระบบไม่ได้
- **AC08:** Receiver / Forklift ทดสอบทำงานหลักบนมือถือจริงได้โดยปลอดภัย
- **AC09:** กรณีเน็ตหลุด ระบบไม่แสดงข้อมูลที่ยังไม่ได้ซิงก์ว่าเป็นยอดกลางที่ยืนยันแล้ว

เกณฑ์เชิงเวลาหรือเปอร์เซ็นต์ต้องเก็บ baseline จริงก่อนกำหนด

## 10. Important Risks

| Risk | Consequence | Proposed control |
|---|---|---|
| มี movement จริงแต่ไม่ได้บันทึก | ยอด/ตำแหน่งไม่ตรง | UX สั้น, ผู้รับผิดชอบที่ชัดเจน, spot check |
| รับ component ไม่ครบ | Overstate จำนวนลังประกอบได้ | เก็บแยก component |
| กระจายหลายพื้นที่/ย้ายของ | ค้นหาไม่เจอ | บันทึก From/To location |
| Offline และหลายคนใช้งาน | sync conflict, double issue | กติกา synchronization และ exception review (TBD) |
| นำข้อมูลบริษัทเข้า Public Repo | ข้อมูลรั่วไหล | ใช้ตัวอย่างสมมติเท่านั้น, ไม่เก็บเอกสารจริง |
| BYOD / public cloud | ไม่ผ่าน IT/security policy | ขออนุญาตก่อนใช้ข้อมูลจริง; auth/RBAC |
| PO 4 ตัวท้ายซ้ำ | ระบุผิดรายการ | ใช้ supplier/invoice/วันที่ประกอบ (ต้องยืนยัน) |

## 11. Definition of Done

- [ ] Acceptance Criteria ผ่านพร้อมหลักฐาน
- [ ] ทดสอบกับผู้ใช้/มือถือจริง; ไม่ใช้ Build success เป็นผล UAT
- [ ] ยอดสต๊อก/ธุรกรรมถูกต้อง และมีวิธีแก้ความคลาดเคลื่อน
- [ ] อธิบาย fallback, backup/recovery และหน้าที่ดูแลได้
- [ ] ได้รับการยอมรับจากผู้ใช้และผู้มีอำนาจที่เกี่ยวข้อง

## 12. Current Baseline

**v0.1 — DEFINE (Draft, NOT Ready to Build)**

ขอบเขตแคบเฉพาะ Wooden Box Inventory & Movement ของ Cutting & Packing เน้น Receiving, Locations, Movements, Stock, Damage และ Excel Export ไม่รวม PPIC/Purchasing/QR/ERP. ยังต้องยืนยันข้อมูลและ flow ก่อนเริ่มพัฒนา

## 13. Important Decisions

| ID | Decision | Status | Date |
|---|---|---|---|
| D-001 | V1 จำกัดเฉพาะ Cutting & Packing inventory/movement | CONFIRMED | 2026-10-28 |
| D-002 | ไม่สแกน QR Code/Barcode | CONFIRMED | 2026-10-28 |
| D-003 | ไม่เชื่อม/บริหารแผน PPIC และ PO Purchasing | CONFIRMED | 2026-10-28 |
| D-004 | Receiving ต้องมีรายการ field ตาม F01 รวม PO 4 ตัวท้าย | CONFIRMED | 2026-10-28 |
| D-005 | ต้องมี movement transactions ที่เรียบง่ายและ Excel export | CONFIRMED | 2026-10-28 |
| D-006 | ผู้เกี่ยวข้อง V1 มี Receiver, Supplier, Supervisor, Forklift | CONFIRMED | 2026-10-28 |
| D-007 | ถ้าเลือก Web App ให้ใช้ Cloudflare | CONFIRMED (conditional) | 2026-10-28 |
| D-008 | ใช้ ledger, offline queue และ RBAC แบบใด | PROPOSED / TBD | 2026-10-28 |

## 14. Change History

| Version | Change | Date |
|---|---|---|
| v0.1 draft | ย้ายจาก generic starter เป็น project-specific DEFINE documentation พร้อม scope boundary | 2026-10-28 |

## Decision Gates

### Ready to Build
- [x] ปัญหาและขอบเขตหลักได้รับการนิยาม
- [ ] Field definitions / identity / receiving flow ชัดพอ
- [ ] Movement / stock rules / correction / offline behavior ยืนยันแล้ว
- [ ] Security/IT cloud policy และ ownership ตกลงแล้ว
- [ ] มี acceptance criteria ที่ผู้ใช้เห็นชอบ
- [ ] มี UI/UX interaction baseline ที่ยอมรับแล้ว

### Ready to Release
- [ ] Functional and data-integrity tests PASS
- [ ] Field UAT PASS / deferred explicitly assessed
- [ ] Auth, backup, recovery and fallback ผ่าน
- [ ] เจ้าของระบบรับมอบและยอมรับ
