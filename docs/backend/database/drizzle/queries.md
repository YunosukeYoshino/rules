## Drizzle ORM クエリ規約 (TypeScript)

### 1. クエリの定義

-   クエリは、データベースとのインタラクションを抽象化する関数として定義します。
-   クエリ関数は、`db` インスタンスと必要なパラメーターを受け取ります。
-   クエリ関数は、データベースから取得したデータを返します。

```typescript
import { db } from "@/db";
import { users } from "@/db/schema/users";
import { eq } from "drizzle-orm";

export const getUserById = async (id: string) => {
  return db.select().from(users).where(eq(users.id, id));
};
```

### 2. 型安全

-   Drizzle ORM は、TypeScript との統合により、型安全なクエリを提供します。
-   スキーマ定義から自動的に型が生成されるため、実行時エラーを減らすことができます。

### 3. トランザクション

-   複数のクエリを 1 つのトランザクションで実行するには、`db.transaction` を使用します。
-   トランザクションは、すべて成功するか、すべてロールバックされることを保証します。

```typescript
import { db } from "@/db";
import { users } from "@/db/schema/users";

export const createUsers = async (newUsers: NewUser[]) => {
  return db.transaction(async (tx) => {
    await tx.insert(users).values(newUsers);
  });
};
```

### 4. エラーハンドリング

-   クエリの実行中にエラーが発生した場合は、適切に処理します。
-   エラーをログに記録し、ユーザーにわかりやすいメッセージを表示します。
