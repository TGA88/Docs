## Workspace
คือ พื้นที่การทำงานทีรวมSourceCode หลายๆ Project ที่มีความเกี่ยวข้องกันไว้ที่เดียวกัน  และ จะจัดเก็บ  1 workspace ไว้ที่ repositry แล้วแต่ยี่ห้อ เช่น github,gitlab เป็นต้น  
การจัดเก็บ Code ไว้ที่ repository จะแบ่งเป็น 2 รูปแบบหลัก คือ 
- Single Project
- Multiple Project 

**สำหรับ Workspace จะเป็นการเก็บ แบบ Multiple project per Repo หรือที่เรียก ว่า Mono Repo**

**มีข้อดีคือ**
- ที่ไม่ต้อง checkout code Prjoect หลายๆที่มาและลดขั้นตอนการเตรียม project ทีจะรัน ระบบ 
- ง่ายต่อการ setup test หลายๆ service component ที่มีความเกี่ยวข้องกัน เพราะ ทุกส่วน อยู่ที่ Workspace เดียวกัน ซึ่งง่ายและลดระยะเวลา ต่อการเตรียมการ  สำหรับการ รันที่ Local ของ Dev แต่ละคน

**ข้อเสีย**
 - เรื่องของ ขนาดrepo ที่ใหญ่ และ computetime ที่อาจจะใช้เวลานานกว่าแบบ Sigle repo บน CI Server ซึ่งต้องบริการจัดการให้ดี

---
## ประเภท ของ Workspace
เราจะแบ่ง Workspace เป็น 2ประเภท คือ

- Global workspace
- System workspace

เพื่อให้เห็น ภาพมากขึ้น เราจะใช้ภาพนี้ เพื่อให้เห็น ภาพรวมโครงสร้าง 
![image](assets/Screenshot%202567-10-27%20at%2011.50.46.png)
### Global workspace
จะเอาไว้ เก็บ Project ที่ต้องการ Reuse การใช้งาน ใน แต่ละ System Workspace

เช่น proejcts
 - fos-ui-common
 - fos-ui-cms (กรณีที่ต้องการทำ Share UI-Component ระหว่าง Lib features ของ CMS ซึ่งอาจจะใช้ ข้ามSystem workspace ก็ได้ในอนาคต)
 - fos-ui-foundation (styes,theme,icons,fonts)
 - fos-ui-react-hooks
 - fos-ui-state-redux
 - fos-ui-logic
 - fos-api-logic
 - fos-api-fastify-plugin

### System workspace
คือ Workspace ที่จะประกอบไปด้วย Project ที่เกียวข้องกับ ระบบ งานนั้น เช่น
ระบบ CMS จะประกอบไปด้วย
Webapp project
    - fos-cms-web
    - fos-cmsadmin-web
Webapi project 
    - fos-cms-webai
    - fos-cmsadmin-webapi
lib projects (อีกหลายๆ Project ที่จะ reuse ใช้งาน เฉพาะ ภานใน Workspace นี้เท่านั้น) เช่น
- scope-webp[frontend]
    - feature-xxx
- scope-webapi[api]
    - [feature]-service
    - [feature]-core
    - [scope]-store-[vendorname]



---

## PNPM Workspace

**เครื่องมือสำหรับการ สร้าง workspace มีอยู่มากมาย เช่น Lerna,NX,NPN,YARN,PNPM**
ซึ่งเราจะเลือกใช้ PNPM มาใช้ในการสร้าง workspace

- Create Workspace
- Config Workspace
- installtion share package for forntend and api project