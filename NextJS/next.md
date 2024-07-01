# NextJS

## NextJS

- React-Base FullStack web development framwork

## Next Rendering methods

1. SSG (static site generation)
    - build 시 미리 생성해놓기
2. SSR (server side rendering)
    - 요청(request)시 생성해서 응답
3. ISR (incremental static regeneration)
    - SSG를 점진적으로 수행(유효기간, 갱신기간 등)
4. CSR (client side rendering)
    - client(brower)에서 JS를 실행하여 화면 처리(SEO에 불리)
    - SSG, SSR, ISR과 조합하여 실행(client component)
    - `‘use client’` 를 붙이면 client에서 렌더링 됨

### React Server Components (RSC)

1. RSC (Server components) - Server
    - default
    - 파일 시스템, http, fetching data from DB
    - hook 사용 안됨
2. RCC (Client components) - Browser
    - ‘use client’를 file 상단에 넣어줌
    - hook 사용 가능

### Next.js 설치

```bash
npx create-next-app@latest

# prettier 설치
npm i -D prettier eslint-config-prettier
npm i -D @trivago/prettier-plugin-sort-imports
```

### Layout & Page

```bash
mkdir app/hello  (Route Folder)
make app/hello/layout.tsx 
make app/hello/page.tsx
nav to page from Home(app/page.tsx) 

yarn dev
yarn build
```