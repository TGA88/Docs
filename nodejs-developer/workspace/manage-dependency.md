# การจัดการ Dependency ใน Workspaces

## การจัดการ Package ระดับ Project


## การจัดการ Libs ที่ใ้ช้ Share เฉพาะใน worksspace

###  shared-web
shared-web คือ scope folder ที่เก็บ project ประเภท fetaure ที่ มีการนำไปใช้ ใน apps หลาย web

โดยจะสร้าง project ภายใต้ folder ui-shared 
```bash
shared-web
|- feature-dummy -> project level packagename = workspacename/feature-dummy
|- featire-funny -> project level packagename = workspacename/feature-funny
```

### shared-webapi
shared-webapi คือ scope folder ที่เก็บ project ประเภท api-core,api-service ที่ มีการนำไปใช้ ใน apps หลาย webapi
```bash
shared-webapi
|- funuy-api-core -> project level packagename = workspacename/funuy-api-core
|- funuy-api-service -> project level packagename = workspacename/funuy-api-service
|- dimmy-api-core -> project level packagename = workspacename/dimmy-api-core
|- dimmy-api-service -> project level packagename = workspacename/ dimmy-api-service
```

## วิธีการ ดู ว่า Lib ไหนควรอยู่ใน Scope Share
คือ การ ดู ที่ package.json ของ web,webapi ใน apps ว่า มันการ ติดตั้งมากกว่า 1 ที่มั้ย เช่น 
> funny-api-core,funny-api-service มีการ ติดตั้งที่ funny-webapi และ foo-webapi ดังนั้นควรย้าย project ไป ภายใต้ scope shared-webapi 

### ควรเขียน script เพื่อช่วยในการตรวจ ย้าย projects ไปไวใน folder scope share 
โดย ตรวจ packages ใน apps  ทั้งหมดว่า มี local package ตัวไหนที่ซ้ำกันบ้าง และ ทำการบ้าย project folder ไปอยู่ ใน scope shared-web หรือ shared-webapi พร้อมทั้ง ปรับ config ให้เหมาะสม เพื่อที่จะ สะดวก ในการนำไปทำ ci-pipeline กรณีที่ จำเป็น ต้อง split ci ตาม apps เพือแก้ปัญหา  รัน ci นาน