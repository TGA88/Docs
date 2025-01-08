## ขั้นตอนการวิเคราะ และ ออกแบบ ระบบ 
หลังจาก ผู้ที่มีหน้าที่ในการ เก็บรวมรวมและสรุปสิ่งที่ลูกค้าอยากได้ (PO/BA/SA ขึ้นอยู่กับองกร กำหนดว่าเป็นหน้าที่ตำแหน่งไหน) แล้ว จะทำการบันทึก เป็น Business Process flow, Activity Process flow แล้ว ดำเนินการ ตามขั้นตอน ต่างๆ ดังนี้ ให้เรียบร้อย ก่อน เริ่มเข้ากระบวนการ พัฒนา หรือ ผลิต

req collective => Biz-Process/Biz-Feature flow => activity/userstory flow=> Action/Biz-Function flow => UseCase and Acceptance => WireFrame and Test Senarios => Continuous Confirmation

Project => Az-Epic
BizProcessItem => Az-Feature
    - ActivityProcessFlow 
        - ActivityProcessItem/หรือที POชอยเรียก UserStory => จะเป็นแค่ รหัส เพื่อนำไปรวมกับ Az-PBI เนื่องจาก เราAdapt ใช้ Az-Epic เป็น Project แทน ทำให้ Level ของ โตรงสร้างไม่พอ
            - ActionItem => Az-PBI (ประกอบด้วย UseCase+Acceptance)
                - UseCase => Az-Task

```
AZ-Org = Team
|- Az-Project = Module/system (AuditHub)
    |- Az-Epic =  Biz-Feature (AuditProfile)
        |- Az-Feature = Biz-UserStory/ActivityItem (MakeProfile)
            |- Az-PBI = Biz-Function/ActionItem (Create,List,Filter,..etc)
                |- Az-Task = Program-Function (unit-test,api,ui)
```

แต่ถ้า JIRA จะมีความลึกแบบนี้
```
Project 
    |- Theme = Module/system (Audithub)
        |- epic = Biz-Feature(AuditProfile)
            |- Story =  Biz-UserStory/ActivityItem (MakeProfile)
                |- task = Biz-Function
                    |- subtask = Program Function
```

### Business Process flow

---

### Activity Process flow (some times is called UserJourney)

---

### Confirm Activity Flow and Acceptances Condition before creating UserStory
---

### Create UserStory(Activity Process) and UseCase(UI Flow) and Acceptances Condition

---

### Convert UseCase as WireFrame

---

### Convert Acceptances Condition of UseCase as Test Senarios

---


### Define UseCase as XP-UserStory and Test Senarios 

Test Senarios  คือ การขยายความ Acceptances Condition ให้มีรายละเอียดเพิิ่มขึ้น เพื่อใช้สื่อสารให้เห็นภาพชัดเจน ระหว่าง AgileTeam กับ PO และ User

---

### Confirm with User for each UseCase of UserStory with Test Senarios

---


### Convert WireFrame to UI HighQuality

---

### Usability Test UI HighQuality

---


### To product backlog refinement to adding detail 

- Define and adding detail for FrontEnd and API from TestSenario and TestCase
- Create ERD
- Create API-Spec
- Create API Task
- Create UI Task and Function Flow spec 

---

