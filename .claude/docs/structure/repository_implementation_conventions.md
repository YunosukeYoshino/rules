## リポジトリ実装の規約

### 1. ファイル構成

- リポジトリインターフェースは /domain/repositories ディレクトリに配置
- リポジトリ実装は /infrastructure/repositories ディレクトリに配置

```typescript
// domain/repositories/userRepository.ts
import { User } from '@/db/schema/users';

export interface UserRepository {
  findById(id: string): Promise<User | null>;
  findByEmail(email: string): Promise<User | null>;
  findAll(): Promise<User[]>;
  create(user: Omit<User, 'id' | 'createdAt' | 'updatedAt'>): Promise<User>;
  update(id: string, data: Partial<User>): Promise<User | null>;
  delete(id: string): Promise<boolean>;
}
```

```typescript
// infrastructure/repositories/d1UserRepository.ts
import { eq } from 'drizzle-orm';
import { db } from '@/db';
import { users } from '@/db/schema/users';
import { UserRepository } from '@/domain/repositories/userRepository';
import type { User, NewUser } from '@/db/schema/users';

export class D1UserRepository implements UserRepository {
  async findById(id: string): Promise<User | null> {
    const result = await db.select().from(users).where(eq(users.id, id)).limit(1);
    return result[0] || null;
  }

  async findByEmail(email: string): Promise<User | null> {
    const result = await db.select().from(users).where(eq(users.email, email)).limit(1);
    return result[0] || null;
  }

  async findAll(): Promise<User[]> {
    return db.select().from(users);
  }

  async create(userData: Omit<User, 'id' | 'createdAt' | 'updatedAt'>): Promise<User> {
    const result = await db.insert(users).values(userData).returning();
    return result[0];
  }

  async update(id: string, data: Partial<User>): Promise<User | null> {
    const result = await db.update(users)
      .set({ ...data, updatedAt: new Date() })
      .where(eq(users.id, id))
      .returning();
    return result[0] || null;
  }

  async delete(id: string): Promise<boolean> {
    const result = await db.delete(users).where(eq(users.id, id));
    return !!result;
  }
}
```

### 2. クエリの実装

- Drizzle の型安全なクエリビルダーを使用
- パフォーマンスを考慮した適切なインデックス設計
- トランザクションを使用した一貫性の確保
