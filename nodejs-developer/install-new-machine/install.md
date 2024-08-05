
## เตรียมเครื่องใหม่สำหรับ พัฒนา program ด้วย NodeJs: ติดตั้ง WSL2, Ubuntu 20.04, NVM และใช้งาน Docker Desktop บน Windows 10/11

### บทนำ

บทความนี้จะแนะนำขั้นตอนการติดตั้งและใช้งาน WSL2 (Windows Subsystem for Linux 2), Ubuntu 20.04, NVM (Node Version Manager) และ Docker Desktop บนระบบปฏิบัติการ Windows 10 และ 11 โดยเน้นการใช้คำสั่ง (CLI) เพื่อให้ผู้ใช้งานสามารถควบคุมและปรับแต่งระบบได้อย่างเต็มที่

### ขั้นตอนที่ 1: ตรวจสอบและเตรียมความพร้อมระบบ 

**(Windows 11: รองรับ WSL2 ตั้งแต่เปิดตัว) ให้ที่หัวข้อถัดไปได้เลย**

* **ตรวจสอบเวอร์ชัน Windows:** WSL2 รองรับ Windows 10 เวอร์ชัน 2004 ขึ้นไป และ Windows 11 ทุกเวอร์ชัน
* **เปิดใช้งาน Virtual Machine Platform:** เข้าไปที่ Settings > Update & Security > For developers แล้วเปิดสวิตช์ "Virtual Machine Platform" **และรีสตาร์ทเครื่อง**

**ถ้า cmd ไม่รู้จัก WSL ให้ ปิด Virtual Machine Platform ใน Turn on Windown feature และ restart**
 จากนั้น ติดตั้งใหม่ด้วย CLI แทน(เมือรันเสร็จแล้ว ให้ restart ก่อนดำเนินการขึ้นถัดไป)
 ```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
 ```

* **อัปเดต Windows:** ตรวจสอบและติดตั้งอัปเดต Windows ล่าสุด



### ขั้นตอนที่ 2: ติดตั้ง WSL2

1. **เปิด PowerShell ในฐานะ Administrator**

2. **อัปเดต WSL:**
   ```powershell
   wsl --install
   ```
3. **ตั้งค่าเวอร์ชันเริ่มต้น:**
   ```powershell
   wsl --set-default-version 2
   ```

### ขั้นตอนที่ 3: ติดตั้ง Ubuntu 20.04

1. **เปิด Microsoft Store**
2. **ค้นหาและติดตั้ง "Ubuntu 20.04 LTS"**
3. **เปิดดิสทริบิวชัน Ubuntu:** ค้นหาในเมนู Start

**หรือ ใช้ CLI**

-  **เพื่อ List Distro Version:**
   ```powershell
   wsl --list --online
   ```
-  **ติดตั้ง Distro Version ที่ต้องการ (Ubuntu-20.04 คืิอ ที่มีใน List):**
   ```powershell
   wsl --install -d Ubuntu-20.04
   ```
- **Starts the default distribution in the Linux user's home directoryl**
   ```powershell
   wsl ~
   ```
- **restart WSL service in poweshell**
   ```powershell
   Get-Service LxssManager | Restart-Service
   ```

   
   

### ขั้นตอนที่ 4: อัปเดตและปรับปรุง Ubuntu

1. **อัปเดตแหล่งข้อมูล:**
   ```bash
   sudo apt update
   ```
2. **อัปเกรดแพ็คเกจ:**
   ```bash
   sudo apt upgrade
   ```

### ขั้นตอนที่ 5: ติดตั้ง NVM

ให้ เปิด Uubuntu และ ดำเนินการ ตามนี้
1. **ดาวน์โหลด script ติดตั้ง:**
   ```bash
   curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
   ```
2. **โหลด NVM:**
   ```bash
   source ~/.bashrc
   ```
3. **ติดตั้ง Node.js latest version:**
   ```bash
   nvm install node
   ```
   **หรือ ติดตั้ง Node.js แบบ ระบุ version ตัวอย่างลง version20:**
   ```bash
   nvm install 20
   ```

### ขั้นตอนที่ 6: ติดตั้ง Docker Desktop

1. **ดาวน์โหลดและติดตั้ง Docker Desktop** จากเว็บไซต์ทางการ
2. **เปิด Docker Desktop**

### ขั้นตอนที่ 7: เชื่อมต่อ Docker กับ WSL2

1. **เปิด Settings ของ Docker Desktop**
2. **เลือก "Use WSL integration"**
3. **เลือกดิสทริบิวชัน Ubuntu**

### การใช้งาน NVM และ Docker Desktop

* **NVM:**
  * **ตรวจสอบเวอร์ชัน Node.js:** `nvm list` **หรือ ตรวจสอบ avaliable verison บน remote** `nvm ls-remote`
  * **เปลี่ยนเวอร์ชัน Node.js:** `nvm use <version>`
  * **ติดตั้งเวอร์ชัน Node.js ใหม่:** `nvm install <version>`
  * **ถอนการติดตั้ง**  `nvm uninstall <version>`
  * **กำหนด default version** `nvm alias default <version>`
  * **คำสั่งเพิ่มเติม** [ดูรายละเอียด](nvm-command.md)
* **Docker:**
  * **สร้าง Docker Image:** `docker build -t my-image .`
  * **สร้าง Docker Image แบบ ระบุ File:** `docker build -f path/Dockerfile -t my-image .`
  * **รัน Docker Container:** `docker run -it my-image`
  * **รัน Docker Container แบบ ระบุชื่อ Container:** `docker run --name <container-name> -it my-image`

### ตัวอย่างการใช้งาน

```bash
# สร้างโปรเจค Node.js ใหม่
mkdir my-node-app
cd my-node-app
npm init -y

# ติดตั้ง Express.js
npm install express

# สร้างไฟล์ app.js
touch app.js

# เขียนโค้ด Node.js ในไฟล์ app.js
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Hello, World!');
});

app.listen(port, () => {
  console.log(`Example app listening at http://localhost:${port}`);
});

# สร้าง Dockerfile
touch Dockerfile

# เขียน Dockerfile
FROM node:14-alpine
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "app.js"]

# สร้าง Docker Image และรัน Container
docker build -t my-node-app .
docker run -p 3000:3000 my-node-app
```

### สรุป

บทความนี้ได้แนะนำขั้นตอนการตั้งค่าสภาพแวดล้อมสำหรับการพัฒนาซอฟต์แวร์โดยใช้ WSL2, Ubuntu, NVM และ Docker Desktop บน Windows 10/11 ผู้ใช้งานสามารถนำความรู้ที่ได้ไปประยุกต์ใช้ในการพัฒนาแอปพลิเคชันต่างๆ ได้อย่างหลากหลาย
