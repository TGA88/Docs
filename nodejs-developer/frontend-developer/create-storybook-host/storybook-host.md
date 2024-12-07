## Setup Storybook (at project level)

- craete project folder
- installation packages
- config project
    - msw
    - storybook
    - tailwind
    - mui
    - eslint
- create stories file to render at storybook-host-example
  

### Create Folder Structure

``` bash
# create proejct folder
# at node-app/
mkdir -p storybook-host/example
mkdir -p storybook-host/example/public
mkdir -p storybook-host/example/assets
cd storybook-host/example
pnpm init

```

### Install Packages
 
copy config ตามด้านล่าง และ แก้ไขname เป็นชื่อ package ที่ต้องการ ในบทความนี่จะใช้ @feedos-example-system/storybook-host-example
```json
// libs/storybook-host/example/package.json
{
  "name": "@your-scope/storybook-host-example",
  "version": "1.0.0",
  "scripts": {
    "storybook": "storybook dev -p 6006",
    "build-storybook": "storybook build"
  },
  "dependencies": {
    "react": "^18.0.0",
    "react-dom": "^18.0.0"
  }
}
```
> แก้ไข @your-scope/storybook-host-example เป็น @feedos-example-system/storybook-host-example แล้ว รันคำสั่ง pnpm install

ให้ copy config ดังนี้ เพิ่มใน package.json ของ root workspace เพราะ ต้องการ share package storybook สำหรับ file .stories.ts ของ libs projecType feature กับ projectType storybook-host-example 
```json
  "devDependencies": {
    "@storybook/addon-essentials": "^8.0.0",
    "@storybook/addon-interactions": "^8.0.0",
    "@storybook/blocks": "^8.0.0",
    "@storybook/react": "^8.0.0",
    "@storybook/react-vite": "^8.0.0",
    "@storybook/test": "^8.0.0",
    "@storybook/testing-library": "^0.2.2",
    "msw-storybook-addon": "^2.0.4",
    "storybook": "^8.0.0",
    "msw": "^2.0.0", // ติดตั้งไ้ว้ที่ root workspace แล้ว
    "vite": "^5.0.0" // // ติดตั้งไ้ว้ที่ root workspace แล้ว
  }
```
หลัง copy config เรียบร้อยแล้วให้ run คำสั่ง
```bash
# at root workspace
pnpm install
```

### Config Project
#### config msw
```bash
# instal worker script for browser    
npx msw init public/ --save
# จะได้ file public/mockServiceWorker.js สำหรับ ใช้ mock service worker ใน browser mode
```

#### config storybook
```bash
# pwd is node-app/storybook-host/example/

# สร้าง folder .storybook
mkdir -p .storybook

# สร้าง file main.ts สำหรับ ???
touch .storybook/main.ts

# สร้าง file preview.ts สำหรับ ??
tocuh .storybook/preview.ts

```
copy config ดังนี้ ใส่ file .storybook/main.ts
```typescript
import type { StorybookConfig } from '@storybook/react-vite'

const config: StorybookConfig = {
  framework: '@storybook/react-vite',
  stories: [
    '../../feature-*/src/**/*.stories.@(js|jsx|ts|tsx)',
  ],
  addons: [
    '@storybook/blocks',
    '@storybook/addon-essentials',
    '@storybook/addon-interactions',
    '@storybook/test' ,
    'msw-storybook-addon'  // เพิ่ม msw addon
  ]
}

export default config
```

copy config ดังนี้ ใส่ file .storybook/preview.ts
``` typescript
import type { Preview } from '@storybook/react'
import { initialize, mswLoader } from 'msw-storybook-addon'

// Import handlers
import { handlers as exmHandlers } from '../../../libs/feature-exm/src/mocks/handlers'


// Initialize MSW
initialize()

export const parameters = {
  actions: { argTypesRegex: "^on[A-Z].*" },
  msw: {
    handlers: [
      ...exmHandlers
    ]
  },
  controls: {
    matchers: {
      color: /(background|color)$/i,
      date: /Date$/,
    },
  },
     // เพิ่ม configuration สำหรับ testing
     interactions: {
      disable: false,
    },
};


export const loaders=[mswLoader]

``` 

    - tailwind
    - mui
    - eslint

### สร้าง Story file ที่ Project feature สำหรับนำไปใช้ render ที่ project storybook-host-example

