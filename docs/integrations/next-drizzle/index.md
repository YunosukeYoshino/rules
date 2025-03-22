## Next.js + Drizzle ORM 統合規約 (TypeScript)

### 1. データベース接続

-   Next.js アプリケーションから Drizzle ORM を使用してデータベースに接続します。
-   データベース接続は、シングルトンパターンを使用して、アプリケーション全体で共有します。

```typescript
// lib/db.ts
import { drizzle } from 'drizzle-orm/libsql';
import { createClient } from '@libsql/client';

const client = createClient({ url: process.env.DATABASE_URL!, authToken: process.env.DATABASE_AUTH_TOKEN });
export const db = drizzle(client);
```

### 2. スキーマ定義

-   Drizzle ORM を使用して、データベーススキーマを定義します。
-   スキーマ定義は、TypeScript で記述します。

```typescript
// db/schema/users.ts
import { sqliteTable, text, integer } from 'drizzle-orm/sqlite-core';

export const users = sqliteTable('users', {
  id: text('id').primaryKey(),
  name: text('name').notNull(),
  email: text('email').notNull(),
});
```

### 3. クエリの実行

-   Drizzle ORM を使用して、データベースに対してクエリを実行します。
-   クエリは、TypeScript で記述します。

```typescript
import { db } from "@/lib/db";
import { users } from "@/db/schema/users";

export const getAllUsers = async () => {
  return db.select().from(users);
};
```

### 4. コンポーネントでの利用

-   Next.js コンポーネントで、Drizzle ORM を使用してデータベースからデータを取得し、表示します。

```typescript
// app/users/page.tsx
import { getAllUsers } from "@/lib/api/users";

export default async function UsersPage() {
  const users = await getAllUsers();

  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
