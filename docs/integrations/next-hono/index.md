## Next.js + Hono 統合規約 (TypeScript)

### 1. API ルートの定義

-   Hono を使用して、Next.js アプリケーションの API ルートを定義します。
-   API ルートは、`app/api` ディレクトリに配置します。

```typescript
// app/api/route.ts
import { Hono } from "hono";

export const app = new Hono();

app.get("/", (c) => {
  return c.json({ message: "Hello Hono!" });
});

export default app;
```

### 2. ランタイムの設定

-   API ルートのランタイムを `edge` に設定します。
-   これにより、API ルートが Vercel Edge Functions で実行されるようになります。

```typescript
export const runtime = 'edge';
```

### 3. データの取得

-   Next.js コンポーネントから Hono API ルートにデータを取得します。
-   `fetch` API を使用して、データを取得します。

```typescript
// app/page.tsx
async function getData() {
  const res = await fetch("http://localhost:3000/api");
  const data = await res.json();
  return data;
}

export default async function Page() {
  const data = await getData();

  return (
    <div>
      <h1>{data.message}</h1>
    </div>
  );
}
```

### 4. ミドルウェアの利用

-   Hono のミドルウェアを Next.js API ルートで使用します。
-   認証、ロギング、CORS などのミドルウェアを利用できます。

```typescript
// app/api/route.ts
import { Hono } from "hono";
import { cors } from 'hono/cors';

export const app = new Hono();

app.use(cors());

app.get("/", (c) => {
  return c.json({ message: "Hello Hono with CORS!" });
});

export default app;
