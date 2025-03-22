## Drizzle ORM + Hono 統合規約 (TypeScript)

### 1. Hono コンテキストの拡張

-   Drizzle ORM を使用するために、Hono コンテキストを拡張します。
-   これにより、すべてのルートハンドラーで `db` インスタンスにアクセスできるようになります。

```typescript
// types/context.d.ts
import { Context } from 'hono';
import { Database } from 'drizzle-orm/sqlite';

type Bindings = {
  DB: Database;
}

export type CustomContext = Context<Bindings>;
```

### 2. ミドルウェアの追加

-   Drizzle ORM の `db` インスタンスを Hono コンテキストに追加するミドルウェアを作成します。

```typescript
// middleware/db.ts
import { drizzle } from 'drizzle-orm/libsql';
import { createClient } from '@libsql/client';
import { MiddlewareHandler } from 'hono';

const client = createClient({ url: process.env.DATABASE_URL!, authToken: process.env.DATABASE_AUTH_TOKEN });
const db = drizzle(client);

export const dbMiddleware: MiddlewareHandler = async (c, next) => {
  c.set('DB', db);
  await next();
};
```

### 3. ルートハンドラーでの利用

-   拡張された Hono コンテキストを使用して、ルートハンドラーで Drizzle ORM クエリを実行します。

```typescript
// routes/users.ts
import { Hono } from 'hono';
import { db } from "@/lib/db";
import { users } from "@/db/schema/users";
import { CustomContext } from '../../types/context';

const usersRoutes = new Hono();

usersRoutes.get('/', async (c: CustomContext) => {
  const users = await c.get('DB').select().from(users);
  return c.json(users);
});

export default usersRoutes;
```

### 4. 型定義の共有

-   Drizzle ORM のスキーマ定義と型定義を、Hono アプリケーション全体で共有します。
-   これにより、型安全なクエリとデータ操作が保証されます。
