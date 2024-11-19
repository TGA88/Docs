## การเตรียมพร้อม สำหรับ NodeJS Developer
- สร้าง Workspace (pnpm+nx)
    - global-workspace
    - system-workspace
- ติดตั้ง Tools ที่จำเป็น สำหรับ Frontend และ Api developer
- สร้าง และ config Project แต่ละ ประเภท สำหรับ Frontend developer
    - สร้าง APP Project 
        - web (Nextjs)
        - visualize UI and Features Component(Storybook)
    - สร้าง Library 
        - feature
        - *ui-foundations(icons,font,CssVariable เช่น colors,space,size สำหรับ share ใช้ ไปท่ี่ ui-mui,ui-tailwind เพื่อ create theeme)
        - *ui-mui(เก็บ component ที่Customจาก mui ,style,theme)
        - *ui-tailwind(เก็บ component ที่Customจาก tailewind ,style,theme, preset)
        - *ui-core(Base Lib**ห้ามมี3rd party lib นอกจาก native lib ของreact** -types,interface,context, สำหรับทุกระบบ ที่ไม่เกี่ยวกับ theme หรือ style)
        - *ui-core-nextjs(impletation componentต่างสำหรับ Nextjs เช่น NextRouterAdapter ที่ implement ReouterAdapterInterface จาก ui-core)
        - *ui-core-remix
        - *ui-react-hooks
        - ui-state-redux
        - *ui-logic
        - mock-api
- สร้าง และ config Project แต่ละ ประเภท สำหรับ API developer
    - สร้าง API Project
        - webapi (fastify)
        - webcron (fastify+cron-plugin)
        - webpub (fastify+sns-plugin)
        - websub (fastify+sqs-plugin)
    - สร้าง Libary
        - service
        - core
        - store (prisma,sequilze,database-driver and etc.)
        - *api-logic
        - *api-core(Base Lib**ห้ามมี3rd party lib นอกจาก native lib** จะเก็บ types,interface,abstract class, สำหรับทุกระบบ เช่น pub-sub,queue,fileStorage)
        - *api-core-aws(จะ implement interace ,abstract class จาก api-core เช่น implement publisher,consumer client ด้วย sns,sqs,fileStorage ด้วย S3)
        - *api-fastify-plugin (custom-fastify-plugin)

## การจัดการ version
- การ จัดการ version ของ publishable libary
- การ จัดการ version ของ app project

## การทำ  Database Migration
- การ ทำ Database migration
    - liquibase
    - prisma-migration

## การ ทำ autmation test(functional)

- overview test ในแต่ละระดับ (unit,integration,component,e2e)
- frontend-test
    - unit-test (test only domain logic) 
        - ประกอบด้วย project type (ui-logic)
    - integration-test (feature-logic)
    - mocking api
- api-test
    - unit-test (test only domain logic)
        - ประกอบด้วย project type service-logic,core-logic,api-logic,api-fastify-plugin
    - integration-test (test ระดับ api-project)
        - test route
        - test db
        - test contract response
    - mocking external service or dependency
- qa-test
    - component-test (seanrios-test api with postman )
    - create mock-server (postman-mock-collection,mockoon or Mountebank )
    
---
## การทำ Code review ด้วย sonar qube
    - install sonar server,scanner and configuration
### Frontend Developer
