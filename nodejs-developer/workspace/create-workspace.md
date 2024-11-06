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
 - fos-ui-common(components,style,theme,icons,fonts)
 - fos-ui-common-tailwind(base on tailwin)
 - fos-ui-cms (กรณีที่ต้องการทำ Share UI-Component ระหว่าง Lib features ของ CMS ซึ่งอาจจะใช้ ข้ามSystem workspace ก็ได้ในอนาคต)
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
- scope-web[frontend]
    - feature-xxx
- scope-webapi[api]
    - [feature]-service
    - [feature]-core
    - [scope]-store-[vendorname]

---

## PNPM Workspace

**เครื่องมือสำหรับการ สร้าง workspace มีอยู่มากมาย เช่น Lerna,NX,NPN,YARN,PNPM**
ซึ่งเราจะเลือกใช้ PNPM มาใช้ในการสร้าง workspace



### สร้าง workspace stucture
```bash

WORKSPACE_DIR='feedos-example-workspace'
SYSTEM_DIR='fos-psc-system'

# สร้าง workspace folder folder 
# mkdir -p <workspace_dir>/node-app/<system_name>
mkdir -p $WORKSPACE_DIR/node-app/$SYSTEM_DIR

mkdir -p $WORKSPACE_DIR/node-app/$SYSTEM_DIR/apps

mkdir -p $WORKSPACE_DIR/node-app/$SYSTEM_DIR/libs

```

### init git
```bash
git init

# create .gitignore file
cat > .gitignore << EOF
# common use git ignore
# See http://help.github.com/ignore-files/ for more about ignoring files.

# compiled output
dist
tmp
/out-tsc

# dependencies
node_modules

# IDEs and editors
/.idea
.project
.classpath
.c9/
*.launch
.settings/
*.sublime-workspace

# IDE - VSCode
.vscode/*
!.vscode/settings.json
!.vscode/tasks.json
!.vscode/launch.json
!.vscode/extensions.json

# misc
/.sass-cache
/connect.lock
/coverage
/libpeerconnection.log
npm-debug.log
yarn-error.log
testem.log
/typings

# System Files
.DS_Store
Thumbs.db

# Next.js
.next

# release
release
release/**
release-app
release-app/**

# storybook
storybook-static
# nx
.nx
node-app/nx-cache
nx-cache

# pnpm
.pnpm-store
.pnpm-store/**

coverage

deploy

.npmrc

data

# Infrastructure
infrastructure/vol/*
.nx/installation
.nx/cache
.nx/workspace-data
EOF


```

### config pnpm

```bash

# กำหนดใช้ pnpm เป็น packageManager
corepack enable pnpm

cd feedos-example-workspace/node-app/fos-psc-system

pnpm init

# set minimum node version
npm pkg set engines.node=">=20"

# set packageManager ใน package.json file
npm pkg set packageManager="pnpm@9.1.4" 

# upgrade packageManager to latest version
corepack use pnpm@latest

```

### config pnpm workspace

```bash
# pwd is $WORKSPACE_DIR/node-app/$SYSTEM_DIR/
cat > pnpm-workspace.yaml << EOF
# pnpm-workspace.yaml
packages:
  # all packages in sub dirs of apps/
  - 'apps/**'
  # all packages in sub dirs of libs/
  - 'libs/**'
  # exclude packages that are inside test directories
  - '!**/test/**'
EOF
```





https://dev.to/vinomanick/create-a-monorepo-using-pnpm-workspace-1ebn


