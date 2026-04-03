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
    %% สร้างโหนด (Nodes)
    A(["1. Discord Trigger (รอรับไฟล์ PDF จากผู้ใช้)"])
    B["2. Extract from File (สกัดตัวอักษรจากไฟล์ PDF)"]
    C{"3. HTTP Request (ส่งให้ Gemini API สรุปเนื้อหา)"}
    D["4. Code Node (หั่นข้อความไม่ให้เกิน 1,900 ตัวอักษร)"]
    E(["5. Discord: Send Message (ส่งข้อความสรุปกลับเข้าห้องแชท)"])

    %% ทิศทางการไหลของข้อมูล (Edges)
    A -->|ส่งไฟล์ PDF| B
    B -->|ส่งข้อความดิบ Text| C
    C -->|ได้บทสรุปยาวๆ| D
    D -->|ส่งข้อความที่หั่นแล้ว| E

    %% กำหนดสไตล์ (Class Definitions)
    classDef discordNode fill:#5865F2,stroke:#ffffff,stroke-width:2px,color:#ffffff,rx:10px,ry:10px;
    classDef processNode fill:#F4A261,stroke:#ffffff,stroke-width:2px,color:#ffffff,rx:10px,ry:10px;
    classDef aiNode fill:#2A9D8F,stroke:#ffffff,stroke-width:2px,color:#ffffff,rx:10px,ry:10px;
    classDef codeNode fill:#E9C46A,stroke:#ffffff,stroke-width:2px,color:#333333,rx:10px,ry:10px;

    %% นำสไตล์ไปผูกกับโหนด (Assign Classes)
    class A,E discordNode;
    class B processNode;
    class C aiNode;
    class D codeNode;