# Research Takeaways — Adapted to Agreed V1 Scope

> บันทึกข้อสังเคราะห์จาก Deep Research ที่เคยทำในวงกว้าง ต่อมาเจ้าของโครงการลด scope ลงเฉพาะ Cutting & Packing inventory/movement
>
> **ไม่ใช่รายงานวิจัยฉบับเต็ม** และไม่ใช่คำสั่งให้สร้าง feature เพิ่ม; สิ่งที่ขัดกับ PROJECT.md ถูกตัดออกแล้ว

## Principles worth keeping

1. **Capture at the point of work:** คนรับ/ย้าย/เบิกเป็นผู้บันทึกเหตุการณ์ที่เกิดจริง ไม่ต้องผ่านคนกลาง
2. **Minimize re-entry:** ระบบคำนวณยอดจาก transactions, ใช้ defaults, auto timestamp/identity เมื่ออนุญาต, เลี่ยงกรอกซ้ำ
3. **Components before complete-box count:** partial receipts ต้องไม่ถูกตีความว่าได้ครบชุด
4. **Location matters:** Move เปลี่ยนตำแหน่งโดยไม่เปลี่ยน total inventory
5. **Exception visibility:** แยกของเสีย/ของดี และมีทางตรวจสอบความคลาดเคลื่อน
6. **Mobile usability:** ปุ่มและคำอธิบายภาษาไทย ใช้ได้กับคนต่างระดับทักษะ
7. **Evidence before scale:** ทดลองกับงานจริงอย่างจำกัดก่อนขยาย

## Explicitly rejected / beyond authority for V1

- QR/Barcode scanning, labels with scanning workflow
- PPIC production plan, Purchasing PO/ETA, automatic material readiness
- ERP integration, supplier procurement/cost dashboards
- Complex WMS optimization, AI OCR, fully automated allocation

## Technology review (still TBD)

- หากใช้ Web App ให้พิจารณาเฉพาะ Cloudflare ตามทิศทางที่ผู้ใช้กำหนด แต่ **ไม่ตัดสินใจ Pages/Workers/D1/Access จนกว่าจะทราบนโยบาย IT**
- Internet อาจขาดช่วง; offline storage/sync/conflict เป็น design decision ไม่ใช่ feature ที่ยืนยันแล้ว
- Security ต้องพิจารณา least privilege, login, safe export, auditing, backup/restore และการใช้ BYOD

## Further reading (reference only)

- [MDN — Progressive Web Apps](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps)
- [MDN — IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
- [OWASP — Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)
- [W3C WCAG — Labels or Instructions](https://www.w3.org/WAI/WCAG22/Understanding/labels-or-instructions.html)
- [Microsoft — Cycle counting concepts](https://learn.microsoft.com/en-us/dynamics365/supply-chain/warehousing/cycle-counting)

**Takeaway:** ลดภาระการบันทึกด้วย process redesign + event-based stock truth ก่อนเลือกเทคโนโลยีเต็มรูปแบบ
