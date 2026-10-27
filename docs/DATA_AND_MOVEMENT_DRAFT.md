# Data & Movement Draft — Wooden Box

> **Status:** Field รายการ Receiving มาจากข้อกำหนดที่ยืนยันแล้ว; โครงสร้างตารางและ business rules เป็น **ข้อเสนอสำหรับการออกแบบ** ไม่ใช่ Database Schema ที่อนุมัติ

## 1. Receiving Fields

| Field | ความหมาย | สถานะ |
|---|---|---|
| Description | รายการ/คำอธิบายลัง | CONFIRMED |
| SKU | รหัส SKU (ชนิดรหัสยัง TBD) | CONFIRMED |
| Sheets per Box | จำนวนแผ่นกระจกต่อลัง | CONFIRMED |
| Glass Size | ขนาดกระจกที่ใส่ในลัง (รูปแบบข้อมูลยัง TBD) | CONFIRMED |
| Top Qty | จำนวนฝาบนที่รับจริง (pcs) | CONFIRMED |
| Bottom Qty | จำนวนฝาล่างที่รับจริง (pcs) | CONFIRMED |
| Side Qty | จำนวนฝาข้างที่รับจริง (pcs) | CONFIRMED |
| PO Last 4 | หมายเลข PO เฉพาะสี่ตัวท้าย เก็บเป็นข้อความ | CONFIRMED |
| Invoice Number | เลขที่ใบส่งของ/Invoice ตามการเรียกของผู้ใช้ (ต้องยืนยันชื่อเอกสาร) | CONFIRMED |
| Receiving Date | วันที่รับจริง | CONFIRMED |
| Receiving Location | พื้นที่รับลง/จุดจัดเก็บ ณ เวลารับ | CONFIRMED |
| Receiver | ชื่อผู้รับ | CONFIRMED |
| Remark | หมายเหตุ เช่น ขาด เกิน เสียหาย หรือค้างส่ง | CONFIRMED |

**Items to confirm:** Supplier (เลือก A/B), Thickness, Grade, Customer, Interleaf Material (กระดาษ/ถุงพลาสติก), Glass Width × Length, damaged quantity, photo attachment, receive time, จำนวนที่แจ้งบนเอกสาร เทียบจำนวนจริง. สิ่งเหล่านี้ถูกกล่าวถึงใน Discovery แต่ไม่ได้ยืนยันว่าทุกช่องต้องบังคับกรอก

**Data-entry goal:** พิจารณา auto-date / auto-user จากบัญชี login, เลือกรายการที่เคยกรอก, ใช้ช่องตัวเลขขนาดใหญ่, เพิ่มหลาย SKU lines ใน Invoice เดียวกัน โดยต้องผ่านการยืนยัน flow กับพนักงานก่อน

## 2. Suggested Logical Structure (PROPOSED)

- **Receipt Header:** internal_receipt_id, supplier, invoice_number, po_last4, received_at, receiver, header_remark
- **Receipt Line:** receipt_id, line_no, SKU/box_spec, description snapshot, sheets_per_box, glass dimensions, top_qty, bottom_qty, side_qty, receive_location, line_remark
- **Location:** location_id, ชื่อพื้นที่, active/inactive (อาจเป็นพื้นที่ชั่วคราว)
- **Stock Position:** key ตาม SKU/ส่วนประกอบ/พื้นที่/สภาพ/lot หรือ receiving line ตามกติกาที่จะตกลง
- **Movement Transaction:** movement_id, type, SKU/component, qty, from_location, to_location, operator, event_time, remark, created_at
- **User/Role:** operator identification, allowed actions (ต้องตกลงร่วมกับ IT)
- **Adjustment/Count:** เฉพาะหากยืนยันว่าต้องใช้สำหรับ reconciliation

รายการนี้ยังไม่รวม full purchase order, PPIC plan หรือ Supplier portal

## 3. Movement Behavior (simple first)

| Event | ผลต่อ usable total | ผลต่อตำแหน่ง | สถานะ |
|---|---|---|---|
| Receive | + จำนวนรับที่ใช้ได้ | เพิ่มในจุดรับ | CONFIRMED capability |
| Move | ไม่เปลี่ยน | หักที่เดิม/เพิ่มที่ใหม่ | CONFIRMED capability |
| Issue / Use | - จำนวนเบิก | หักจากที่เก็บ | CONFIRMED capability |
| Damage | ลด usable หากเสียหาย | เก็บข้อมูลชำรุด/แยกสถานะ | CONFIRMED capability |
| Count / Adjust | +/- ตามการแก้ยอดหลังตรวจสอบ | เปลี่ยนยอดตามพื้นที่ | PROPOSED / TBD |
| Reverse / Correction | ย้อนรายการผิดแบบตรวจสอบได้ | ตาม event ที่แก้ | PROPOSED / TBD |

**PROPOSED integrity rules:**
1. ไม่ควรแก้ยอดคงเหลือทับตรง ๆ โดยไม่มีเหตุการณ์อ้างอิง
2. การย้ายของต้องไม่เพิ่มหรือลดยอดรวม
3. ห้ามบันทึกซ้ำเมื่อ network retry หรือกดปุ่มยืนยันซ้ำ
4. ควรตรวจยอดที่มีอยู่จริงก่อน Issue/Move และแสดงข้อขัดแย้งอย่างชัดเจน
5. ยอดชำรุดไม่ควรถูกนับใน usable balance
6. กรณี offline ห้ามแสดงรายการที่ยัง sync ไม่สำเร็จว่าได้รับยืนยันจากส่วนกลาง

## 4. Capacity Example (hypothetical)

ต่อ 1 ลัง: Top ×1, Bottom ×1, Side ×2. หากมี Top 50, Bottom 40, Side 100:

**Max theoretical complete boxes = min(50, 40, floor(100/2)) = 40 boxes**

**Caution:** นี่เป็นความสามารถเชิงจำนวนเท่านั้น; หากชิ้นส่วนอยู่คนละพื้นที่ มีของเสีย หรือความเข้ากันได้ของสเปกไม่ตรงกัน จำนวนที่หยิบใช้ได้จริงอาจต่ำกว่า

## 5. Identity / Consistency Decisions Required

- SKU เป็นรหัสของลังหรือของกระจก? ควรมี Box Spec ID อีกตัวหรือไม่?
- PO สี่หลักท้ายไม่ unique: ใช้คู่กับอะไรเพื่อค้นหารายการ?
- 1 Invoice มีหลาย SKU และหลาย receiving location ได้หรือไม่?
- จะติดตามชิ้นส่วนแยกตาม Lot/Receipt หรือรวมยอดตาม SKU + Location?
- Move/Issue เป็นรายชุด (bundle) หรือรายจำนวนชิ้นส่วน?
- มีการเบิกครั้งละบางส่วน/แยกชิ้นหรือไม่? หากใช่ต้องออกแบบให้รองรับ
- การรับ damaged: เก็บ count แยกจาก good ตั้งแต่แรกหรือบันทึก Damage event?
- ผู้ใช้ต้องการไฟล์ .xlsx หรือ .csv ที่ Excel อ่านได้?
- ผู้ใช้หลายคน offline พร้อมกันต้องทำอย่างไร?

## 6. Excel Export (required capability, format TBD)

**PROPOSED datasets:** Receiving Records, Current Stock by SKU/Component/Location, Movement History, Damage/Adjustment History (ถ้าอยู่ใน scope ที่ตกลง)

**Keep:** วันที่เป็น ISO format, PO last4 เป็น text เพื่อรักษาเลขศูนย์นำหน้า, จำนวนเป็น integer ในหน่วย pcs, column headers เป็นภาษาไทยหรือไทย/อังกฤษตามการตัดสินใจผู้ใช้

**Security:** ส่งออกเฉพาะข้อมูลที่สิทธิ์ผู้ใช้อนุญาต ไม่ใส่ Invoice/ลูกค้าจริงเป็นตัวอย่างใน Public Repo
