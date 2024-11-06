## ติดตั้ง เครืองมือสำหรับใช้ในการพัฒนา และ Package ที่ใช้ร่วมกัน ระหว่าง Frontend และ API Projects

- [NX Setup](#nx-setup)
  - [install and config nx tasks and cache](#ติดตั้ง-nx-และ-init-config)
- commitizen (conventional commit)
- eslint
    - frontend(react,react-hooks)
    - api (only typescript)
    - jest-plugin
- prittier
- typescript
- jest
- storybook
- msw


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
## Setup eslint