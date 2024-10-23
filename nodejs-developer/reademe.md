## การเตรียมพร้อม สำหรับ NodeJS Developer
- ติดตั้ง Tools ที่จำเป็น สำหรับ Frontend และ Api developer
- สร้าง Workspace (pnpm+nx)
    - global-workspace
    - system-workspace
- สร้าง และ config Project แต่ละ ประเภท สำหรับ Frontend developer
    - สร้าง APP Project 
        - web (Nextjs)
        - visualize UI and Features Component(Storybook)
    - สร้าง Library 
        - feature
        - *ui-component
        - *ui-foundation
        - *ui-react-hooks
        - ui-state-redux
        - *ui-logic
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

### Frontend Developer
