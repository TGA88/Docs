## ติดตั้ง เครืองมือสำหรับใช้ในการพัฒนา และ Package ที่ใช้ร่วมกัน ระหว่าง Frontend และ API Projects

- [NX Setup](#nx-setup)
  - [install and config nx tasks and cache](#ติดตั้ง-nx-และ-init-config)
- [git-cz (conventional commit) ](#ติดตั้ง-committzen) 
- [typescript](#setup-typescript)
- [Tsup](#setup-tsup)
- [jest](#setup-jest)
- [prittier](#setup-prettier)
- [eslint](#set-eslint)
- [vite และ plugin](#setup-vite-and-rollup)
  - @vitejs/react
  - @node-exteral
  - @dts
  - @reserve-decorative
- [msw](#setup-msw)



- create base config
    - jest
    - tsconfig
    - tsup

---
### NX Setup
คือ Tools ที่เอาไว้จัดการ Project แบบ Mono-Repo ที่มีประสิทธิภาพ ซึ่งจะมี 2 รูปแบบ คือ 
- Project Base คือ Nx จะสร้าง Project file (project.json) และ มี Template generator Project ประเภทต่างๆมาให้พร้อมใช้งานเลย 
- Package Base คือ เราจะต้องสร้าง Project เอง แต่ จะยังคงใช้ความสามารถ Tasks Pipeline,Task caching และ Dependcy graph ได้ 

ซึ่งเราจะเลือกใช้ แบบ Packaage Base เพราะว่า แบบ Project base มีความยุ่งยาก กรณีที่เราต้องการ Customize Project Temaplate และ Config 

#### ติดตั้ง NX และ init config
```bash
# ติดตั้ง NX ที่ระดับ Project
pnpm install nx -D -w

# initial nx เพื่อ สร้าง config file nx.json
pnpx nx@latest init
```

option ในการ init ให้เลือก ดังนี้
```bash
# option ในการ init ให้เลือก ดังนี้
? Would you like remote caching to make your build faster? …  
(it's free and can be disabled any time)
Yes
Skip for now <-- ให้เลือกอันนี้
```

เมือเสร็จสิ้นจะได้ file  nx.json ที่ root workspace
```json
{
  "$schema": "./node_modules/nx/schemas/nx-schema.json",
  "defaultBase": "master"
}
```
จากนั้นจะทำการ Set Nx tasks และ Cache ที่จำเป็น ดังนี้
- serve ไว้สำหรับ run serve application
- lint ไว้สำหรับ run lint application กับ library และใช้ cache
- test ไว้สำหรับ run test application กับ library และใช้ cache
- build ไว้สำหรับ run build application กับ library และใช้ cache
```bash
#  add nx command
npm pkg delete scripts.test

npm pkg set scripts.lint="nx lint"
npm pkg set scripts.lint:all="nx run-many --target=lint --all"
npm pkg set scripts.test="nx test"
npm pkg set scripts.test:all="nx run-many --target=test --all"
npm pkg set scripts.build="nx build"
npm pkg set scripts.build:all="nx run-many --target=build --all"
npm pkg set scripts.release="nx run-many --target=build --all"
npm pkg set scripts.release:all="nx run-many --target=release --all && nx run-many --target=release-storybook --all"



 

```

**ให้แก้ไขfile nx.json ดังนี้**
dependsOn หมายถึง การรัน target นี้จะต้องรัน target อื่นที่เป็น dependency ก่อน 
```json
{
  "$schema": "./node_modules/nx/schemas/nx-schema.json",
  "tasksRunnerOptions": {
    "default": {
      "runner": "nx/tasks-runners/default",
      "options": {
        "cacheableOperations": [
          "build",
          "lint",
          "test"
        ]
      }
    }
  },
  "targetDefaults": {
    "serve": {
      "dependsOn": [
        "build",
        "^build",
        "^serve"
      ]
    },
    "lint": {
      "dependsOn": [
        "^lint"
      ],
      "inputs": ["default"],
      "cache": true
    },
    "test": {
      "dependsOn": [
        "^build"
      ],
      "inputs": ["default"],
      "cache": true,
      "outputs": [
        "{workspaceRoot}/coverage/{projectRoot}/"
      ]
    },
    "build": {
      "dependsOn": [
        "^build"
      ],
      "inputs": ["production","^production"],
      "outputs": [
        "{projectRoot}/dist/", 
        "{projectRoot}/storybook-static/"
      ],
      "cache": true
    },
    "release": {
      "dependsOn": [
        "^build",
        "build"
      ]
    }
  },
  "namedInputs": {
    "default": ["{projectRoot}/**/*", "sharedGlobals"],
    "production": [
      "default",
      "!{projectRoot}/**/?(*.)+(spec|test).[jt]s?(x)?(.snap)",
      "!{projectRoot}/tsconfig.spec.json",
      "!{projectRoot}/jest.config.[jt]s",
      "!{projectRoot}/src/test-setup.[jt]s",
      "!{projectRoot}/test-setup.[jt]s",
      "!{projectRoot}/.eslintrc.json",
      "!{projectRoot}/eslint.config.js"

    ],
    "sharedGlobals": [
    "!{projectRoot}/node_modules/",
    "!{projectRoot}/dist/",
    "!{projectRoot}/coverage/",
    "!{projectRoot}/.next/",
    "!{projectRoot}/storybook-static/",
    "!{projectRoot}/next-env.d.ts"

  ]
  },
  "defaultBase": "main"
}
```
> (เครื่องหมาย '^' เช่น ^build หมายถึงรัน คำสั่งbuild ที่ project ที่เป็น dependecy ก่อน build project target)

---

### ติดตั้ง Committzen
ช่วยใน การ เขียน commit message ตาม Conventianl commit

```bash
# add package as devDependcies at root workspace
pnpm add git-cz -W -D

# set command to commit
npm pkg set scripts.commit="git-cz"
```
สร้าง file changelog.config.js ที่ working directory ของ editor 
```javascript
// changelog.config.js
module.exports = {
    disableEmoji: true,
    format: '{type}{scope}: {emoji}{subject}',
    list: ['feat', 'fix', 'test', 'chore','build', 'docs', 'refactor', 'style', 'ci', 'perf'],
    maxMessageLength: 72,
    minMessageLength: 3,
    questions: ['type', 'scope', 'subject', 'body', 'breaking', 'issues', 'lerna'],
    scopes: [
        "example-web",
        "example-webapi",
        "example-webcron",
        "example-webpub",
        "example-websub",
        "example1-api",
        "example2-api",
        "example-store-prisma",
        "example-webconfig",
        "feature-example1",
        "feature-example2",
        "ui-state-redux",
    ],
    types: {
        chore: {
            description: 'Build process or auxiliary tool changes',
            emoji: '🤖',
            value: 'chore'
        },
        build: {
            description: 'edit configuration of build tools or add/remove dependency',
            emoji: '⚙️',
            value: 'build'
        },
        ci: {
            description: 'CI related changes',
            emoji: '🎡',
            value: 'ci'
        },
        docs: {
            description: 'Documentation only changes',
            emoji: '✏️',
            value: 'docs'
        },
        feat: {
            description: 'A new feature',
            emoji: '🎸',
            value: 'feat'
        },
        fix: {
            description: 'A bug fix',
            emoji: '🐛',
            value: 'fix'
        },
        perf: {
            description: 'A code change that improves performance',
            emoji: '⚡️',
            value: 'perf'
        },
        refactor: {
            description: 'A code change that neither fixes a bug or adds a feature',
            emoji: '💡',
            value: 'refactor'
        },
        release: {
            description: 'Create a release commit',
            emoji: '🏹',
            value: 'release'
        },
        style: {
            description: 'Markup, white-space, formatting, missing semi-colons...',
            emoji: '💄',
            value: 'style'
        },
        test: {
            description: 'Adding missing tests',
            emoji: '💍',
            value: 'test'
        },
        messages: {
            type: 'Select the type of change that you\'re committing:',
            customScope: 'Select the scope this component affects:',
            subject: 'Write a short, imperative mood description of the change:\n',
            body: 'Provide a longer description of the change:\n ',
            breaking: 'List any breaking changes:\n',
            footer: 'Issues this commit closes, e.g #123:',
            confirmCommit: 'The packages that this commit has affected\n',
        },
    }
};
```
---
## Setup typescript
เราจะ setup tsconfig.json เพื่อเอาไว้ให้ editor อย่าง vscode ใช้ validate typechecking and syntax ตอน ที่เราเขียน code เท่านั้น ส่วนการ  build จะ ใช้ package ที่ชื่อ tsup ในการ compile 
> **ยกเว้นProjectType ที่ใช้ fastify จะใช้ tsc เนื่องจาก fastify-cli ยังไม่ลองรับ compiler ตัวอื่นนอกจาก tsc**

#### ติดดั้ง และ init typescript
```bash
pnpm add -Dw typescript@latest

pnpm tsc --init
```
#### สร้าง tsconfig.base.json สำหรับ Share config ให้ base tsconfig ของ project type ประเภทต่างๆใน Workspace 
- create tsconfig.base.json

#### สร้าง tsconfig base แต่ละ project type ของ system-workspace
- create tsconfig-web.base.json
- create tsconfig-webapi.base.json
- create tsconfig-feature.base.json
- create tsconfig-ui-state-redux.base.json
- create tsconfig-api-service.base.json
- create tsconfig-api-core.base.json
- create tsconfig-api-store.base.json
- create tsconfig-api-client.base.json
#### สร้าง tsconfig base แต่ละ project type ของ global-workspace(shared-packages)
- create tsconfig-ui.base.json
- create tsconfig-ui-core.base.json
- create tsconfig-ui-react-hooks.base.json
- create tsconfig-share-logic.base.json

---

## Set Type สำหรับ sub project ประเภทต่าง

```bash 
# install type สำหรับ sub project type  nodejs
pnpm add -Dw @types/node

# install type สำหรับ sub project type  ui-component และ features
pnpm add -Dw @types/react @types/react-dom
# 
```

---

## Setup Tsup
Tsup เป็นเครื่องมือสำหรับ build TypeScript โดยเฉพาะ ที่เน้นความเร็วและใช้งานง่าย มาดูข้อดีของ Tsup กัน:

1. ความเร็ว:
- ใช้ esbuild เป็น bundler ทำให้ build ได้เร็วมาก
- มี watch mode สำหรับ development

2. ง่ายต่อการใช้งาน:
- ตั้งค่าน้อย ส่วนใหญ่มีค่า default ที่เหมาะสมอยู่แล้ว
- รองรับ TypeScript โดยตรง ไม่ต้องตั้งค่าเพิ่มเติม

3. ความสามารถ:
- สร้าง bundle ได้ทั้ง ESM และ CommonJS
- รองรับ Tree-shaking 
- มี minification ในตัว
- split code อัตโนมัติ

เหมาะสำหรับ:
- การสร้าง npm packages
- การ build TypeScript libraries
- โปรเจคที่ต้องการความเร็วในการ build
- ต้องการ setup ที่ง่าย ไม่ซับซ้อน

```bash
#ระบุให้ใช้ version 6.6.0 เพราะ version 8 มีปัญหา เรื่อง javascript heap out of memory ในกรณีที่ มี file มากเกินในกาน compile 1 ครั้ง 
pnpm add -Dw tsup@^6.6.0
```
#### ทำการสร้าง file tsup config เอาไว้ที่ root workspace เพื่อเอาไว้ใช้ ใน แต่ ละ project type

```typescript
// tsup.lib.config
import { defineConfig } from 'tsup';

export default defineConfig({
  entry: ['src/index.ts', 'src/**/*.ts', 'src/**/*.tsx','!**/*test*/**'],
  format: ['cjs', 'esm'],
  splitting: true,
  sourcemap: true,
  clean: true,
  minify: true,
  dts: true,
  outDir: 'dist',
});

```

---

### Setup Jest

```bash

# install jest และ eslint เพื่อให้ ทุก project type สามารถใช้ jest ได้
pnpm add -Dw jest @types/jest ts-jest

# Jest: 'ts-node' is required for the TypeScript configuration files.
pnpm add -Dw ts-node

# intall testing tools สำหรับ Next.js หรือ feature project ที่ต้องใช้ renderHooks เพื่อ Test CustomHooks
pnpm add -Dw jest-environment-jsdom @testing-library/react @testing-library/jest-dom @testing-library/user-event identity-obj-proxy

```

create jest.config.ts ที่ root workspace
```typescript
// jest.config.ts
import type { Config } from 'jest';
import { defaults } from 'jest-config';

const baseConfig: Config = {
  preset: 'ts-jest',
  verbose: true,
  moduleFileExtensions: [...defaults.moduleFileExtensions],
  transform: {
    '^.+\\.tsx?$': [
      'ts-jest',
      {
        isolatedModules: true,
        diagnostics: {
          ignoreCodes: ['TS151001']
        }
      }
    ]
  },
  moduleNameMapper: {
    '^@/(.*)$': '<rootDir>/src/$1'
  },
  collectCoverageFrom: [
    'src/**/*.{ts,tsx}',
    '!src/**/*.d.ts',
    '!src/**/*.stories.{ts,tsx}',
    '!src/**/index.{ts,tsx}',
    '!src/**/*.test.{ts,tsx}'
  ],
  coverageDirectory: 'coverage',
  coverageReporters: ['text', 'lcov', 'json-summary'],
  coverageThreshold: {
    global: {
      branches: 80,
      functions: 80,
      lines: 80,
      statements: 80
    }
  }
};

export default baseConfig;
``` 

---

### Setup Prettier

```bash
# install prettier at root workspace
pnpm add -Dw prettier 

npm pkg set scripts.format:all="nx run-many --target=format --all"
npm pkg set scripts.format-check:all="nx run-many --target=format-check --all"
```
---

## Set EsLint
Eslint คือ tools ที่ช่วยในการ ตรวจสอบ code syntax เพือลดช้อผิดพลาด และ ช่วยให้ developer ทุกคน เขียน code ภายใต้ Code standard เดียวกัน ซึ่งจะทน้าที่หลัก 2 ส่วน คือ 
- Code Qulity rule (code standard ต่างๆ การตั้งชื่อตัวแปร การตั้งชื่อ function เป็น ต้น) 
- Formatter rule (เช่น จบคำสั่งด้วย ; หรือ , เป็น ต้น และที่ Linter ต้องมี formatter เพื่อไว้ fix code ให้เป็นไปตาม quality rule) 
> เราจะใช้ Eslint เฉพาะในส่วน ของ Quality rule เท่านั้น ส่วน การ formatter จะใช้ prittier แทน  

```bash 
#  install at root workspace
pnpm add -Dw eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin @eslint/js

# globals เอาไว้ใช้ set environment ใน eslint เพื่อให้รู้ว่า จะต้อง ignore keyword document,localstorage เมื่อ set environment เป็น globals.browser เป็นต้น
pnpm add -Dw globals


#install eslint plugin สำหรับ react,nextjs ถ้าใน workspace มี project type ใช้  nextjs,react หลายๆProject
pnpm add -Dw @next/eslint-plugin-next eslint-plugin-react eslint-plugin-react-hooks

# install jest และ eslint เพื่อให้ ทุก project type สามารถใช้ jest ได้
pnpm add -Dw eslint-plugin-jest

# install และ config prittier ให้ disable rule ที่ซ้ำกับ eslint
pnpm add -Dw eslint-config-prettier 


```
สร้าง file config ไว้ที่ root workspace (จะใช้ config รูปแบบ Flat ซึ่งจะรองรับ ตั้งแต่ v8.21.0เป็นต้นไป)

```typescript
// Flat config  System for eslint version 8.x above
// root-eslint.config.mjs
import js from '@eslint/js';
import tseslint from '@typescript-eslint/eslint-plugin';
import * as tsParser from '@typescript-eslint/parser';
import prettier from 'eslint-config-prettier';
import globals from 'globals';

export function createBaseConfig({ tsConfigPath = './tsconfig.json' } = {}) {
  return [
    // JavaScript base config
    js.configs.recommended,

    // TypeScript base config
    {
      // กำหนดว่าจะใช้กับไฟล์อะไรบ้าง
      files: ['**/*.{ts,tsx,mts,cts}'],
      // กำหนด TypeScript parser และ options
      languageOptions: {
        parser: tsParser,
        parserOptions: {
          // project => ไม่รองรับการใช้ References ใน tsconfig จะต้อง อ้างถึง tsconfig.xx.json ตัวที่ระบุfileที่ ต้องการให้ eslint ตรวจ
          // project: tsConfigPath,
          // projectService วิธีนี้รองรับ References ใน tsconfig
          projectService: {
            cwd: process.cwd(),
            skipLoadingLibrary: true,
            matchingStrategy: 'recursive',
          },
          ecmaVersion: 2022,
          sourceType: 'module',
          ecmaFeatures: {
            jsx: true,
          },
        },
        // เพิ่ม globals สำหรับ browser environment
        globals: {
          ...globals.browser, // จะได้ window, document, localStorage, etc. ทำให้ eslint ไม่ฟ้อง error
          ...global.jest
        },
      },
      // กำหนด plugins
      plugins: {
        '@typescript-eslint': tseslint,
      },
      // กำหนด rules
      rules: {
        // ปิด ESLint rule ที่ conflict กับ @typescript-eslint
        'no-unused-vars': 'off',

        // TypeScript rules
        '@typescript-eslint/no-explicit-any': 'warn',
        '@typescript-eslint/explicit-function-return-type': 'off',
        '@typescript-eslint/no-unused-vars': [
          'error',
          {
            argsIgnorePattern: '^_',
            varsIgnorePattern: '^_',
          },
        ],
      },
    },

    // Config สำหรับไฟล์ configs ที่เป็น TypeScript
    {
      files: ['**/*.config.{ts,mts}', '**/jest.config.{ts,mts}'],
      languageOptions: {
        parser: tsParser,
        parserOptions: {
          // project: tsConfigPath,
          projectService: {
            cwd: process.cwd(),
            skipLoadingLibrary: true,
            matchingStrategy: 'recursive',
          },
          ecmaVersion: 2022,
          sourceType: 'module',
        },
        globals: {
          ...globals.node, // Config สำหรับ Node.js code (เช่น config files)
        },
      },
    },
    // แยก config สำหรับไฟล์ test โดยเฉพาะ
    {
      files: ['**/*.test.ts', '**/*.test.tsx', '**/*.spec.ts', '**/*.spec.tsx'],
      languageOptions: {
        globals: {
          ...globals.jest, // เพิ่ม jest globals
        },
      },
    },
    // Prettier config (ต้องอยู่ท้ายสุด)
    {
      files: ['**/*.{js,jsx,ts,tsx,mts,cts}'],
      rules: {
        ...prettier.rules,
      },
    },
  ];
}

```

> Flat Config System เป็นการปรับปรุงที่ดีกว่าแบบเดิมในหลายๆ ด้าน โดยเฉพาะเรื่อง performance, type safety และความชัดเจน แต่อาจต้องใช้เวลาในการปรับตัว


---

## Setup Vite and Rollup
สำรหรับ ใช้ build react component libary และ ใช้ Rollup plugin เพื่อช่วยในการทำ Tree shaking
```bash
# install vite และ plugin
pnpm add -Dw vite @vitejs/plugin-react vite-plugin-dts vite-plugin-lib-inject-css vite-plugin-sass-dts

# install Rollup Plugin 
# rollup-plugin-node-externals  สำหรับ unbundle dependency ใน package.json
# rollup-plugin-preserve-directives เพื่อให้ ไม่ถูก remove comment 'use client' หลังจาก bunble
pnpm add -Dw rollup-plugin-node-externals rollup-plugin-preserve-directives

# instll glob สำหรับใช้ ในการ config vite ใน sub project type ui,features
pnpm add -Dw glob
```
---



## Setup Msw

เอาไว้ใช้ Mock Api บน Environment Browser หรือ Nodejs เพื่อให้เป็น อิสระจาก library httpClient อย่าง axios หรือ fetch ทำให้เวลาเปลี่ยน lib จาก fetch ไปใช้ axios แล้ว จะทำให้เราไม้ต้อง แก้ไข mock httpclient 

```bash

# ติดตั้ง MSW ที่ root workspace เพื่อ แชร์ ไปใช้ที่ sub project type ต่างๆ และ จะ init config ที่ ระดับ project 
pnpm add -Dw msw

# ใช้ jest-fixed-jsdom แทน jest-environment-jsdom เพื่อ แก้ปัญหา msw v2 Error Request/Response/TextEncoder is not defined (Jest)
pnpm add -Dw jest-fixed-jsdom



```

---