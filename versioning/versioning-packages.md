## ทำVersioning Package ด้วย Changeset
การทำ versioning package เราแบ่ง เป็น2 รูปแบบ คือ
- Pre-Release version
- Release Stable version

### การทำ Pre Release Version
เราทำ pre release เพื่อที่จะเอาไว้สำหรับใช้ทดสอบ new feature หรือfix bug ให้แน่ใจในความถูกต้องก่อน ที่จะrelase ให้ผู้อื่นนำ code เราไปใช้งาน
> ตัวอย่าง pre-release version 1.0.1-next.1

นอกจาก นี้ การใช้ pre release จะทำให้ package manager อย่าง npm,yarn,pnpm เวลาที่ สั่ง ติดตั้งpackage จะนำเฉพาะ stable ไปติดตั้งเท่านั้น ทำให้ มั่นใจได้ว่า จะไม่เจอ version ที่อาจจะไม่สมบูรณ์ ไปใช้งาน 
**หากต้องการติดตั้ง verison pre-release**
เพื่อนำไปทดสอบก็สามารถทำได้เช่นกัน
> pnpm add packagename@pre-version  
ตัวอย่าง pnpm add @global/ui-common@1.0.1-next.1


### การทำ Stable Version
หลังจาก ที่เรา ทดสอบ feature หรือ fix bug จะมั่นใจแล้วเราจะทำการ bump จาก pre-release เป็น stable เพื่อที่จะrelease version ที่สมบูรณ์ให้ ผู้อื่นใช้ เช่น
> จาก @global/ui-commin@1.0.1-next.1 => @global/ui-commin@1.0.1

### การทำ Versioning ด้วย tools ที่ชื่อว่า changeset
ทำการติดตั้ง package 
```bash
# ติิดตั้ง changeset เป็น devDependcies ที่ workspace
pnpm add -Dw changeset

# init changeset config จะได้ file config.json ภายใต้ folder .changeset
pnpm changeset init
```

#### ทำการ สร้าง pre-release 
```bash
# step1  ทำการ สร้าง feature branch
# git checkout -b [branch-name]
git checkout -b hotfix/v1.0.1

# step2 เปิด pre-release mode จะได้ file pre.json ซึ่งจะเอาไว้ เก็บ change state
pnpm changeset pre enter next

# step3 commit code เพื่อ จะ track จุดที่เปิด pre-release mode 
git add . && git commit -m "build: Enter Pre-release mode and versioning"

# step4 ทำการแก้ไข code และ commit code ต้องการแล้ว

# step5 ทำการ สั่ง bump version changeset จะทำการ bump pre-version และ สร้าง Update changelog file พร้อมทั้งการ commit  ให้
pnpm changeset version

# step6 หลังจาก สั่ง bump version changeset จะ update change state ที่ pre.json แต่ไม่ทำการ commit ให้ เราจึงต้อง commit เอง เพื่อเอาไว้ใช้ ต้องที่ จะ bump stable version
git add . && git commit -m 'build: update pre-version at change-state file'

#  ทำการ publish to npm registry
pnpm publish -r --push-branch hotfix/v1.0.1

# หากต้องการ ทำ pre version ถัดไป ให้ทำการ ตั้งแต่่ step4-7 ใหม่ จะได้ pre-version ถัดไป

```

#### ทำการ สร้าง Stable version
```bash
# at bracnch hotfix/v1.0.1
# ทำการ change mode stable
pnpm changeset pre exit

# ทำการ bump stable version
pnpm changeset version

# หลังจาก bump stable version แล้ว changeset จะทำการ ลบ file pre.json เราจะ ต้อง commit การ delete file.json เข้าgitเอง
git add .  && git commit -m "build: change to stable mode and delete pre release state"

# ทำการ merge เข้า main branch 
git rebase main

# ทำการ publish to npm หรือ จะ ทำการ push remote to main branch เพื่อที่จะ publish stable ด้วย ci ก็ได้
pnpm publish -r

# หลังจาก publishเรียบร้อยแล้ว ให้ทำการ tag stable version เพื่อง่ายในการค้นหา และ delete branch v1.0.1 เพราะไม่จำเป็นต้องใช้แล้ว
git tag @global/ui-common-v1.0.1 && git branch -D hotfix/v1.0.1

```

### Bonus

script สำหรับการ Bump Stable และ Pre-Release version

เพิ่มคำสั่ง script ใน package.json ดังนี้
```json
scripts:{
    "changeset": "changeset",
    "publish": "npm run changeset publish",
    "version:stable": "npm run changeset version",
    "version:pre-version": "npm run changeset version  && git add . && git commit --allow-empty -m 'build: update pre-version state' ",
    "pre-version": "changeset pre enter next && git add . && git commit --allow-empty -m 'build: Enter prerelease mode' ",
    "stable": "changeset pre exit && git commit --allow-empty -m 'build: Enter stable mode and version packages' && pnpm version:stable && git add . && git commit --allow-empty -m 'build: update stable state and delete pre-version state' "
}

```

