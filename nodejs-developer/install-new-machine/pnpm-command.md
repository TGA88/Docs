
## Corepack และ PNPM
- Corepack
- PNPM

## Corepack คืออะไร และทำไมถึงสำคัญ?

**Corepack** เป็นเครื่องมือที่ถูกเพิ่มเข้ามาใน Node.js ตั้งแต่เวอร์ชัน 16.19.0 เป็นต้นมา มีหน้าที่หลักในการจัดการ package manager ต่างๆ เช่น npm, yarn, หรือ pnpm ภายในโครงการ Node.js ของเราเอง ทำให้เราสามารถเลือกใช้ package manager ที่เหมาะสมกับความต้องการของแต่ละโปรเจคได้อย่างยืดหยุ่น

**ทำไมต้องใช้ Corepack?**

* **ความยืดหยุ่นในการเลือก package manager:** ไม่จำเป็นต้องติดตั้ง package manager แต่ละตัวลงในระบบอีกต่อไป เพียงแค่ใช้ Corepack ในการสลับไปมาระหว่าง package manager ต่างๆ ได้เลย
* **การจัดการเวอร์ชัน:** Corepack ช่วยให้เราสามารถกำหนดเวอร์ชันของ package manager ที่ต้องการใช้งานได้อย่างชัดเจน ทำให้มั่นใจได้ว่าทุกคนในทีมจะใช้ package manager เวอร์ชันเดียวกัน
* **การทำงานร่วมกัน:** Corepack ทำให้การทำงานร่วมกันในทีมเป็นไปได้ง่ายขึ้น เพราะทุกคนสามารถใช้ package manager ที่คุ้นเคยได้
* **การ deploy:** สามารถแพ็คเกจ package manager ร่วมกับแอปพลิเคชัน เพื่อให้สามารถ deploy ไปยังเซิร์ฟเวอร์ที่ไม่มีการติดตั้ง package manager ไว้ได้

**หลักการทำงานของ Corepack**

Corepack จะสร้างสภาพแวดล้อมจำลอง (isolated environment) สำหรับแต่ละ package manager ที่เราใช้ ทำให้ package manager แต่ละตัวทำงานแยกกันโดยไม่ขัดขวางกัน

**วิธีการใช้งาน Corepack**

1. **ตรวจสอบเวอร์ชัน Node.js:** ตรวจสอบว่า Node.js ที่คุณใช้อยู่มีเวอร์ชัน 16.19.0 ขึ้นไปหรือไม่
2. **เริ่มต้นใช้งาน Corepack:**
   * **ติดตั้ง package manager:**
     ```bash
     corepack enable <package-manager>
     ```
     ตัวอย่าง:
     ```bash
     corepack enable npm
     corepack enable yarn
     corepack enable pnpm
     ```
   * **เลือก package manager:**
     ```bash
     corepack use <package-manager>
     ```
     ตัวอย่าง:
     ```bash
     corepack use npm
     ```
3. **ใช้งาน package manager:** หลังจากเลือก package manager แล้ว คุณสามารถใช้งานคำสั่งของ package manager นั้นได้ตามปกติ เช่น `npm install`, `yarn install`, `pnpm install`

**ตัวอย่างการใช้งาน:**

```bash
# ติดตั้ง npm
corepack enable npm

# เลือกใช้ npm
corepack use npm

# ติดตั้ง dependency โดยใช้ npm
npm install
```

**ข้อดีเพิ่มเติมของ Corepack**

* **ง่ายต่อการเรียนรู้:** การใช้งาน Corepack นั้นค่อนข้างง่าย เพียงแค่เรียนรู้คำสั่งพื้นฐานของ Corepack ก็สามารถใช้งานได้แล้ว
* **ยืดหยุ่น:** Corepack สามารถใช้กับโครงการ Node.js ทุกรูปแบบ ไม่ว่าจะเป็นโครงการขนาดเล็กหรือขนาดใหญ่
* **เปิดกว้าง:** Corepack รองรับ package manager ที่หลากหลาย ไม่จำเป็นต้องจำกัดอยู่แค่ npm, yarn, หรือ pnpm

**สรุป**

Corepack เป็นเครื่องมือที่ทรงพลัง ช่วยให้เราสามารถจัดการ package manager ได้อย่างมีประสิทธิภาพและยืดหยุ่น ทำให้การพัฒนาแอปพลิเคชัน Node.js เป็นไปได้ง่ายขึ้นและสะดวกสบายมากยิ่งขึ้น

**คำสั่ง เบื้องต้นสำหรับใช้ corepack:**

```bash
# เปิดใช้งาน Corepack
corepack enable

# ติดตั้ง pnpm global 
# ใช้กรณี current path ใน terminal อยู่นอก Project จะใช้global version  
corepack install --global pnpm@<version>

# ใช้ติดตั้ง pnpm ใน project หรือ ต้องการ upgrade pnpm version ของ project
# หลังจาก รัน แล้วcorepack จะระบุ packagemaneger version ใน package.json file เพื่อให้ คนอื่นๆ ใช้ versionเดียวกัน ตามที่ระบุใน package.json
corepack use pnpm@7.x # sets the latest 7.x version in the package.json

# คำสั่งนี้ ใช้เปลี่ยน version pnpm ที่ติดตั้ง ด้วย corepack
# สำหรับ Nodejs verion 18 เป็นต้นไป
corepack prepare pnpm@7.13.6 --activate

# สำหรับ nodejs version 20 เป็นต้นไป
corepack install --global pnpm@7.13.6

# check corepack version 
corepack --version
```

---


## PMPM



```bash
#  check version
pnpm --version

#  add new package to project
pnpm add <packagename> 

# install exists package from lock file into project
# pnpm install จะต่างกับ pnpm add ตรงที่ไม่สามารถใช้ option ในการติดตั้ง package หลายๆ project พร้อมกัน หรือ ระบุproject ที่ต้องการติดตั้ง โดย ที่ ไม่ต้อง cd ไปที่ project นั้นก่อน 
pnpm install <packagename>
 
# remove package from project
pnpm remove <packagename>

# clear all package in content address store
pnpm store prune

# packing project for publishing to npm registry
pnpm publish

# deployment project in workspace 
# จะทำการ สร้าง folder ใหม่ไปที่ destination path และ clone project code และ ทำการ merge node_modules ของ project กับ root workspace ให้หร้อมสำหรับการ deploy 
# หากเราต้องการ bundle file ให้ทำใน folderนี้ เพราะ เป็น sourcecode ที่พร้อมรัน ใน PRD mode
pnpm -F <projectname> --prod deploy <destination path>


# update package

# update specific package

# list package version

```