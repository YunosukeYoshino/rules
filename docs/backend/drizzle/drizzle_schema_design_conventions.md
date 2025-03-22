## Drizzle スキーマ設計の規約

### 1. スキーマ定義

- 各テーブルは別ファイルで定義
- スキーマファイルは /db/schema ディレクトリに配置
- TypeScript の型は自動生成を活用

```typescript
// db/schema/users.ts
import { sqliteTable, text, integer } from 'drizzle-orm/sqlite-core';
import { createId } from '@paralleldrive/cuid2';

export const users = sqliteTable('users', {
  id: text('id').primaryKey().$defaultFn(() => createId()),
  name: text('name').notNull(),
  email: text('email').notNull().unique(),
  createdAt: integer('created_at', { mode: 'timestamp' })
    .$defaultFn(() => new Date()),
  updatedAt: integer('updated_at', { mode: 'timestamp' })
    .$defaultFn(() => new Date()),
});

export type User = typeof users.$inferSelect;
export type NewUser = typeof users.$inferInsert;
```

### 2. リレーション定義

- 外部キー制約を明示的に定義
- リレーションシップは relations オブジェクトで定義

```typescript
// db/schema/posts.ts
import { sqliteTable, text, integer } from 'drizzle-orm/sqlite-core';
import { createId } from '@paralleldrive/cuid2';
import { users } from './users';
import { relations } from 'drizzle-orm';

export const posts = sqliteTable('posts', {
  id: text('id').primaryKey().$defaultFn(() => createId()),
  title: text('title').notNull(),
  content: text('content').notNull(),
  userId: text('user_id').notNull().references(() => users.id),
  createdAt: integer('created_at', { mode: 'timestamp' })
    .$defaultFn(() => new Date()),
  updatedAt: integer('updated_at', { mode: 'timestamp' })
    .$defaultFn(() => new Date()),
});

export const postsRelations = relations(posts, ({ one }) => ({
  user: one(users, {
    fields: [posts.userId],
    references: [users.id],
  }),
}));
```

### 3. マイグレーション

- マイグレーションは Drizzle Kit を使用
- マイグレーションファイルは /db/migrations ディレクトリに配置

```bash
# マイグレーションの生成
bun run drizzle-kit generate

# マイグレーションの実行
bun run drizzle-kit migrate
