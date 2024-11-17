## สร้าง Project สำหรับ Features compoment
สำหรับ เอาไว้ สร้าง component และ logic ที่ใช้ ใน featuresนี้เท่านั้น โดย ที่ Feature Component จะไม่ยึดติดกับ React Framework ใด้ เพื่อที่จะได้ในไปใช้กับ project web ที่จะใช้ Framework อะไรก็ได้ เช่น Nextjs, Remix เป็น ต้น


create project by vite lib mode

install 
- react
- react-dom
- react-query
- mui
- tailwind


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