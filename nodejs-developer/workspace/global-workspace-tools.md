init packages for global


```bash
# install dev tool on root

typesctipt
tsup
vite
eslint
prettier
jest
msw
storybook (สำหรับรัน ui-common)
git-cz

# for GTM
"@next/third-parties": "14.2.12",

# for onBoarding feature
"react-joyride": "^2.9.2",
# customization(position,style) of react-joyride 
"react-floater": "^0.9.4",

# devDependencires (ยังไม่รู้ใช้ทำอะไร) และต้อง config ที่ื nextjs.config ด้วย เพื่อให้ ฺBuild Project Nextjsผ่าน
"raw-loader": "^4.0.2",

# Optional ถ้าต้องใช้
# React PDF Component
"@react-pdf-viewer/core": "^3.12.0",
"@react-pdf-viewer/get-file": "^3.12.0",
"@react-pdf-viewer/print": "^3.12.0",
"@react-pdf-viewer/toolbar": "^3.12.0",

# render power bi
"powerbi-client-react": "^1.4.0",



```

ีui-auth-client (ใช้ได้เฉพาะบน front เพราะมีการใช้ localstorage)
``` 
```

ui (common,customhook-react)
```bash
react
react-dom
```

project 
```bash
สำหรับ ประกาศ type,inteface ของ Base Class,Component ให้ Implementtion package นำไปใช้ เช่น ui-core-nextjs จะนำไป implement router,image เป็นต้น

ส่วน api-core จะเป็น 
```

ui-functions (มีการใช้ builtin เฉพาะ เช่น documents navigation ที่ทำงานได้เฉพาะ web browser
```bash
init only dev-tools at root workspace
```

api-functions (builtin ของ node เช่น fs,path,http/https,process,os ที่ไม่สามรถใช้ได้บน web browser หรือ 3party ที่ใช้ คำสั่งของ nodejs)
```bash
init only dev-tools at root workspace
```

common-functions (pure logic ที่ไม่ได้ใช้ buitin package ของ nodejs หรือ frontlib ต่างๆ)
```bash
init only dev-tools at root workspace
```


fastify-plugin
```bash
fastify
```

