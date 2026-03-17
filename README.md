# 📄 บอทสรุปเอกสาร (Document Summarizer Bot)

บอทอัตโนมัติสำหรับช่วยสรุปใจความสำคัญจากเอกสารหรือข้อความที่ยาวเหยียด สร้างขึ้นเพื่อช่วยประหยัดเวลาในการอ่านและทำความเข้าใจข้อมูล โดยใช้ **n8n** เป็นตัวจัดการ Workflow และ **Gemini API** เป็น AI สำหรับประมวลผล

## 🎯 Problem Statement

* **WHO (ใครเดือดร้อน):** นักเรียน และ นักศึกษา
* **WHAT (ปัญหาคืออะไร):** ผู้ใช้ต้องการทราบเฉพาะประเด็นสำคัญของเอกสาร แต่ในปัจจุบันต้องเสียเวลาอ่านเนื้อหาทั้งหมดก่อนถึงจะจับใจความได้
* **WHEN (เกิดบ่อยแค่ไหน):** เกิดขึ้นทุกครั้งเมื่อต้องทำความเข้าใจเอกสาร หรือบทความที่มีความยาวมาก
* **HOW MUCH (เสียเวลาเท่าไหร่):** เอกสารความยาว 20–30 หน้า มักใช้เวลาอ่านถึง 30–60 นาที

## 💡 Proposed Solution

สร้าง **"บอทสรุปเอกสาร"** ที่สามารถรับข้อความหรือเอกสารเข้ามา แล้วทำการสรุปเนื้อหาสำคัญแบบอัตโนมัติ เพื่อลดเวลาในการอ่านเอกสารลง และช่วยให้ผู้ใช้เข้าใจเนื้อหาหลักได้อย่างรวดเร็วและแม่นยำ

---

## ⚙️ System Architecture (n8n Workflow Concept)

ระบบทำงานผ่าน n8n โดยมีลำดับขั้นตอนดังนี้:

```mermaid
graph TD
    A[ผู้ใช้ / ระบบภายนอก] -->|ส่งข้อความ/เอกสาร| B(Webhook Trigger)
    B --> C[Code Node]
    C -->|จัดเตรียม/แปลงข้อมูล| D{AI Agent}
    D -.->|ส่งคำสั่ง| E((Gemini API))
    E -.->|ส่งผลการสรุป| D
    D --> F{IF Node}
    F -->|สรุปสำเร็จ/เงื่อนไขผ่าน| G[Discord: Notify Summary]
    F -->|เกิดข้อผิดพลาด| H[Discord: Notify Error]
    
    classDef trigger fill:#f9c23c,stroke:#333,stroke-width:2px,color:black;
    classDef process fill:#6b8af2,stroke:#333,stroke-width:2px,color:white;
    classDef ai fill:#a866f2,stroke:#333,stroke-width:2px,color:white;
    classDef notify fill:#5865F2,stroke:#333,stroke-width:2px,color:white;
    
    class B trigger;
    class C,F process;
    class D,E ai;
    class G,H notify;