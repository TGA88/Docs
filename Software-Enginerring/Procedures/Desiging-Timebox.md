# การทำ PBR แต่ ละ Program(เทียบเท่ากับ Story ของ Team)

ประกอบ ด้วย 3ส่วน
- Technical Design
- Test Design
- DataTest Design
- Review Design Spec By Team(~2Hr/Progam)
<br/>

Program 1 ตัว เฉลี่ยจะมีประมาณ 5 Function งาน(ไม่ใช้ coding Function)
โดยเฉลี่ย  1 Program ควรจะใช้เวลา Desgin จบภายใน 1-3 วัน ขึ้นอยู่กับ ขนาด (**1วัน=6ชม**)

- S (5 Function) => 1 วัน 
- M (10 Function) => 2 วัน
- L (15 Function) => 3 วัน

<br/>

ยกตัวอย่างเช่น Program Size S จำนวน เวลาที่จะแบ่ง ให้ กับ Task 3 ประเภท ขึ้นอยู่กับ Requirement ว่า มีรายละเอียด หรือ มีความซับซ้อนมากในส่วนไหน ของ การ Design 3รูปแบบ อาจจะแบ่งเป็น
- Tech Desgin 3ชม (เพราะมีหลาย Component และ ต้องมี การrender หลายแบบ)
- Test Design 2 ชม
- DataTest Design 1 ชม (ใช้เวลาน้อยเพราะ Data อาจจะ Reuseใช้งานได้จาก Actionอื่น )

โดย จะต้องเฉลี่ยเวลา ให้กับ แต่ละ Fucntion หรือ Action(แล้วแต่ทีมเรียก)
เช่น มีเวลาทำ Tech-Design 3ชม สำหรับ5 Function => (3*60)/5 เท่ากับ จะมีเวลา 36นาที ต่อ 1 Function เป็นต้น

<br/>

### Effort รายบุคคล
กรณี ออกแบบ เดียวทั้งProgram
- S 1วัน (6-7 Hr.)
- M 3 วัน (18-21 Hr.)
- L 5 วัน (30-35 Hr.)
<br/>

## ทั้งหมดนี้เป็น แค่ Guideline ไม่มีสูตรสำเร็จในการ แบ่งขนาดของ Program ความยากและง่าย ขึ้นอยู่กับ ทักษะของทีม
