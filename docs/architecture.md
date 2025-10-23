# โครงสร้างโค้ด (Architecture)

ภาพรวมโครงสร้างโฟลเดอร์หลักของโปรเจค (ตัวอย่าง — ปรับตามจริง)

- src/ — โค้ดหลักของแอป
  - api/ — ชั้น API / route handlers
  - services/ — ธุรกิจหลักและ logic
  - models/ — โครงสร้างข้อมูล / ORM models
- assets/ — ไฟล์ static (รูป, แผนที่, icons)
- scripts/ — สคริปต์ช่วยเหลือต่าง ๆ
- docs/ — เอกสาร (ไฟล์ MkDocs นี้)

แนะนำให้เพิ่ม diagram (Mermaid) หรือภาพสถาปัตยกรรมที่อธิบาย data flow และ integration points
