# การตั้งค่าสภาพแวดล้อมสำหรับการพัฒนา

แนะนำขั้นตอนการเตรียม environment สำหรับนักพัฒนา

- ติดตั้งเครื่องมือหลัก:
  - Python 3.11+ (ถ้าใช้)
  - Node.js 18+
  - Docker (ถ้าต้องการรัน service แบบ container)
- สร้าง virtual environment (Python):
  python -m venv .venv
  source .venv/bin/activate
  pip install -r requirements.txt
- คำสั่งที่ใช้บ่อย:
  - รัน server (local): npm start หรือ python app.py
  - รัน unit tests: pytest หรือ npm test
  - ตรวจ static analysis: flake8 / eslint

คำอธิบายเพิ่มเติมเกี่ยวกับ local database, API keys, และ fixtures ให้ดูไฟล์ config.example หรือ environment.md (เพิ่มตามที่โครงการต้องการ)
