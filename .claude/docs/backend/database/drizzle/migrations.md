## Drizzle ORM マイグレーション規約 (TypeScript)

### 1. マイグレーションファイルの生成

-   Drizzle Kit を使用してマイグレーションファイルを生成します。
-   マイグレーションファイルは、データベーススキーマの変更を記述する SQL ファイルです。

```bash
bunx drizzle-kit generate:sqlite
```

### 2. マイグレーションの実行

-   生成されたマイグレーションファイルをデータベースに適用します。
-   マイグレーションは、データベーススキーマを最新の状態に保つために実行されます。

```bash
bunx drizzle-kit push:sqlite
```

### 3. マイグレーションファイルの管理

-   マイグレーションファイルは、バージョン管理システムで管理します。
-   これにより、データベーススキーマの変更履歴を追跡し、必要に応じてロールバックできます。

### 4. シードデータの投入

-   データベースに初期データを投入するには、シードデータファイルを使用します。
-   シードデータファイルは、データベースに投入するデータを記述する SQL ファイルまたは TypeScript ファイルです。

```typescript
import { db } from "@/db";
import { users } from "@/db/schema/users";

export const seed = async () => {
  await db.insert(users).values([
    { name: "John Doe", email: "john.doe@example.com" },
    { name: "Jane Doe", email: "jane.doe@example.com" },
  ]);
};
