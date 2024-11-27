## สร้าง Project สำหรับ Features compoment
สำหรับ เอาไว้ สร้าง component และ logic ที่ใช้ ใน featuresนี้เท่านั้น โดย ที่ Feature Component จะไม่ยึดติดกับ React Framework ใด้ เพื่อที่จะได้ในไปใช้กับ project web ที่จะใช้ Framework อะไรก็ได้ เช่น Nextjs, Remix เป็น ต้น

- create project by vite lib mode
- install package
- config project
    - eslint.config
    - jest.config
    - typescript config
    - vite.config.ts
> ในบมควานนี้ จะต้อง สร้าง pnpm workspace ชื่อ feedos-example-system/ และ Config workspace ให้เรียบร้อยก่อน **viteและplugin ที่จำเป็น** ได้ติดตั้งที่ root เรียบร้อยแล้ว ตอนสร้าง workspace

### create project by vite lib 
เราจะทำการสร้าง sub project ใน pnpm workspace ชื่อ ว่า feature-exm เมื่อสร้างเสร็จ จะได้โครงสร้าง ดังนี้
```

feedos-example-system/
└── workspaces/
    └── node-app/
        ├── .nx/                      # NX workspace cache
        ├── apps/                     # Applications
        ├── libs/                     # Shared libraries
        │   └── feature-exm/          # Example feature library
        │       ├── dist/             # Build output
        │       ├── lib/              # Library source code
        │       ├── node_modules/     # Local dependencies
        │       ├── public/           # Public assets
        │       ├── src/              # Source code
        │       ├── .gitignore        # Git ignore rules
        │       ├── eslint.config.mjs # ESLint configuration
        │       ├── jest.config.ts    # Jest test configuration
        │       ├── package.json      # Package manifest
        │       ├── README.md         # Documentation
        │       ├── tsconfig.build.json # TypeScript build config
        │       ├── tsconfig.json     # TypeScript config
        │       └── vite.config.ts    # Vite build config
        ├── node_modules/             # Workspace dependencies
        ├── storybook-host/          # Storybook configuration
        ├── tools/                   # Development tools
        ├── .prettierignore         # Prettier ignore rules
        ├── .prettierrc             # Prettier configuration
        ├── jest.config.base.ts     # Base Jest config
        ├── jest.config.features.ts # Feature tests config
        ├── nx.json                 # NX configuration
        ├── package.json            # Workspace package manifest
        ├── pnpm-lock.yaml         # PNPM lock file
        ├── pnpm-workspace.yaml    # PNPM workspace config
        └── root-eslint.config.mjs # Root ESLint config
```
ทำการสร้าง sub project
``` bash
# cwd = feedos-example-system/workspaces/node-app/libs/

# สร้าง folder
mkdir -p feature-exm

# สร้าง folder lib สำหรับ ใช้ เก็บ code ทุกอย่าง ที่เราต้องการ publish ขึ้น npm
mkdir -p lib

# สร้าง folder src เพื่อเก็บ code สำหรับ ใช้development(จะไม่publish) เช่น mocks,stories file
mkdir -p src
```


### install packages
จะติดตั้ง packages ที่ใช้เฉพาะ sub projectนี้ 
- react
- react-dom
- react-query

> **mui,tailwind** จะติดตั้งที่ root workspace และ จะconfig ท่ี project ที่จะนำ lib feature-xxx ไปใช้งาน เช่น storybook-host-xxx หรือ exampel-web/nextjsเป็นต้น


```bash
# change directory to project

# add react
pnpm add -D react react-dom

# add @tanstack/react-query/
pnpm add -w @tanstack/react-query

# add mui
pnpm add -Dw tailwindcss postcss autoprefixer 

# add tailwind
pnpm add -w @mui/material @emotion/react @emotion/styled @mui/icons-material @mui/x-data-grid
```

### Config Project

สร้าง file eslint.config.mjs และ copy config ใส่ใน file เพื่อ overwrite base config จาก root workspace ตามนี้

``` typescript
// libs/feature-exm/eslint.config.mjs
import { createBaseConfig } from '../../root-eslint.config.mjs';
import reactPlugin from 'eslint-plugin-react';
import reactHooksPlugin from 'eslint-plugin-react-hooks';
import globals from "globals";

export default [
  ...createBaseConfig({ tsConfigPath: './tsconfig.json' }),

  {
    files: ['**/*.{jsx,tsx}'],
    plugins: {
      'react': reactPlugin,
      'react-hooks': reactHooksPlugin
    },
    settings: {
      react: {
        version: 'detect'
      }
    },
    languageOptions: {
      globals: {
        ...globals.browser,
        // ...globals.commonjs, // ถ้าใช้ CommonJS modules
        JSX: 'readonly' // สำหรับ React JSX
      }
    },
    rules: {
      'react/react-in-jsx-scope': 'off',
      'react/prop-types': 'off',
      'react/display-name': 'error',
      'react-hooks/rules-of-hooks': 'error',
      'react-hooks/exhaustive-deps': 'warn'
    }
  }
];

```

สร้าง file jest.config.ts และ copy config ใส่ใน file เพื่อ overwrite base config จาก root workspace ตามนี้

``` typescript
// libs/feature-exm/jest.config.ts

/** @type {import('jest').Config}  */
import type {Config} from 'jest';
import baseConfig from '../../jest.config.features';

const featureConfig: Config = {
  rootDir: __dirname,
  ...baseConfig
}

export default featureConfig;
```

สำหรับ config typscript จะสร้าง config มา2 file คือ 
- tsconfig.ts => เราเอาไว้ใช้ สำหรับให้ vscode,jest,eslint นำไปใช้งานสำหรับ validation syntax และ typechecking เท่านั้น
- tscongig.build.ts => เราจะเอาไว้ ให้ vite ใช้สำหรับ generate declaration type file (file .d.ts)

สร้าง file **tsconfig.ts** และ copy config ใส่ใน file เพื่อ overwrite base config จาก root workspace ตามนี้

``` typescript
// I am using tsconfig for eslint,jest instead of using refernce path to 
// tsconfig.xx.json file because it's more complex to config 
// and have to config compilationOptions for each file 
// such as tsconfig.eslint.json,tsconfig.test.json , tsconfig.build.json and tsconfig.json for vscode
// references artical "https://www.totaltypescript.com/concepts/option-module-must-be-set-to-nodenext-when-option-moduleresolution-is-set-to-nodenext"
{
  "extends": "../../tsconfig.features.base.json",
  "compilerOptions": {
    // // If you're not bundling with TypeScript with tsc but using esbuild,swc,vite instead.
    // // you should be using moduleResolution: "bundler" and module: "ESNext":
    // "module": "ESNext",
    // "moduleResolution": "bundler",
    // // setting noEmiit for use tsc for typechecking only
    // "noEmit": true,


    "types": [
      "node",
      "jest",
      "@testing-library/jest-dom"
    ]
  },
  "include": [
    "**/*.ts",
    "**/*.tsx",
    "**/*.js",
    "**/*.jsx"
  ]
}
```

สร้าง file **tsconfig.build.ts** และ copy config ใส่ใน file เพื่อ overwrite base config จาก root workspace ตามนี้
``` typescript
{
  "extends": "../../tsconfig.features.base.json",
  "compilerOptions": {
    "moduleDetection": "force",
    "rootDir": "lib"
  },
  "include": [
    "lib"
  ],
  "exclude": [
    "**/*.test.*",
    "**/*.spec.*",
    "**/*.stories.*",
  ]
}
```
สร้าง file **vite.config.ts** และ copy config ใส่ใน file เพื่อ overwrite base config จาก root workspace ตามนี้
```typescript
// vite.config.ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import dts from 'vite-plugin-dts'
import { libInjectCss } from 'vite-plugin-lib-inject-css'
import { nodeExternals } from 'rollup-plugin-node-externals'
import type { Plugin, RenderedChunk, NormalizedOutputOptions } from 'rollup'
import { extname, relative, resolve,dirname } from 'path';
import { fileURLToPath } from 'node:url';
import { glob } from 'glob';


//ใช้เพื่อ preserve declarelative react client component
const preserveUseClientDirective = (): Plugin => {
  return {
    name: 'preserve-use-client',
    renderChunk(
      code: string, 
      chunk: RenderedChunk,
      _options: NormalizedOutputOptions
    ) {
      // เพิ่ม this context
      const moduleInfo = this.getModuleInfo(chunk.facadeModuleId as string);
      const sourceCode = moduleInfo?.code || '';
      
      if (sourceCode.includes("'use client'") || sourceCode.includes('"use client"')) {
        return `'use client';\n${code}`;
      }
      return code;
    }
  };
};

// ใน ESM เราไม่สามารถใช้ __dirname ได้โดยตรง เพราะเป็น CommonJS 
// วิธีที่ 1: ใช้ import.meta.url
const __filename = fileURLToPath(import.meta.url);
const __dirname = dirname(__filename);
// วิธีที่ 2: ใช้ URL และ fileURLToPath
// ตัวอย่าง rootDir: dirname(fileURLToPath(import.meta.url)),
// วิธีที่ 3: ใช้ process.cwd() สำหรับ root directory ของ project
// ตัวอย่าาง   rootDir: process.cwd(),

export default defineConfig({
  plugins: [
    react(),
    // dts({ 
    //   include: ['lib'],
    //   exclude: ['lib/**/*.test.*','lib/**/*.spec.*', 'src/**/*.stories.*'],
    // }),
    dts({tsconfigPath: 'tsconfig.build.json',outDir: 'dist/types', }),
    preserveUseClientDirective(),
    libInjectCss(),
    {
      ...nodeExternals({
        devDeps: true,
        peerDeps: true,
      }),
      enforce: 'pre'
    }
  ],
  build: {
    // lib mode กับ format: 'es' การ minify จะไม่ minify whitespaces เพราะจะทำให้ pure annotations หายและส่งผลต่อ tree-shaking
    lib: {
      entry: resolve(__dirname, 'lib/main.ts'),
      // formats: ['es','cjs'],
      formats: ['es'], //เราจะใช้ เฉพาะ es อย่างเดียว เพราะ react component ของเราจะนำไปใช้กับ รูปแบบ esm อย่างเดียวจึงไม่ต้อง compile format cjs
      // fileName: (format) => `main.${format}.js`
      fileName: (format) => `[name].${format === 'es' ? 'js' : 'cjs'}`
    },
    rollupOptions: {
      input: Object.fromEntries(
        glob
          .sync('lib/**/*.{ts,tsx}')
          .filter((file) => !file.endsWith('.test.ts'))
          .filter((file) => !file.endsWith('.test.tsx'))
          .filter((file) => !file.endsWith('.stories.tsx'))
          .map((file) => [
            // The name of the entry point
            // lib/nested/foo.ts becomes nested/foo
            relative('lib', file.slice(0, file.length - extname(file).length)),
            // The absolute path to the entry file
            // lib/nested/foo.ts becomes /project/lib/nested/foo.ts
            fileURLToPath(new URL(file, import.meta.url)),
          ]),
      ),
      output: {
        assetFileNames: 'assets/[name][extname]',
        // entryFileNames: '[name].js', //ถ้าที่ build.lib มีการ set fileNameแล้ว จะใช้ตรงนั้นเป็น output name โดยที่ไม่ต้องSet entryFileNamesก็ได้
        // preserveModules: true,
        // preserveModulesRoot: 'lib',
        // plugins: [preserveUseClientDirective()]
      }
    }
  }
})

```
 copy config ดังนี้ เพิ่มใน file package.json 

 ```json
 {
   "type": "module",
  "main": "dist/main.js",
  "types": "dist/types/main.d.ts",
  "files": [
    "dist"
  ],
  "sideEffects": [
    "**/*.css"
  ],
  "publishConfig": {
    "access":"restricted" 
  },
  "scripts": {
    "dev": "vite",
    "build": "tsc --p tsconfig.build.json && vite build", 
    "build:showConfig": "tsc --p tsconfig.build.json --showconfig",
    "lint": "eslint . && tsc",
    "preview": "vite preview",
    "test": "pnpm run test:gen ; pnpm run fix:lcov",
    "test:gen": "jest --runInBand --coverage --no-cache",
    "fix:lcov": "bash ../../tools/fix_lcov_paths.sh ../../coverage/libs/feature-exm ",
    "test:show": "jest --runInBand --coverage --showConfig"
  },
 }
 ```

### Conclusion
ตามโครงสร้าง นี้ อาจจะใช้้เวลาในsetup เพราะว่า sub project feature เราอาจจะต้องสร้าง บ่อย ดังนั้น เราได้ เตรียม tools สำหรับ สร้าง ProjectType Feature ไว้แล้ว ดูรายละเอียดได้ที่นี่ [click](../../workspace/workspace-generator.md)