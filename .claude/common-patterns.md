# 共通パターンとテンプレート

## 開発ワークフローパターン

### 1. タスク管理パターン
```markdown
# 複雑なタスクの場合
1. TodoWriteでタスクリスト作成
2. in_progressでタスク開始
3. 完了時にimmediatelyマーク
4. 新しいタスクは前のタスク完了後

# シンプルなタスクの場合
- 直接実行（TodoWrite不要）
```

### 2. ファイル操作パターン
```typescript
// 読み取り → 編集 → 検証の順序
1. Read tool でファイル内容確認
2. Edit/MultiEdit で変更実行
3. 必要に応じてlint/typecheck実行
```

### 3. 検索パターン
```markdown
# キーワード検索
- Task tool: 広範囲な検索
- Grep tool: 特定パターン検索
- Glob tool: ファイル名パターン

# 具体的なファイル
- Read tool: 直接ファイル読み取り
```

## コードテンプレート

### 1. Next.js Componentパターン
```typescript
// Server Component (デフォルト)
interface Props {
  readonly param: string;
}

export default function Component({ param }: Props) {
  return <div>{param}</div>;
}

// Client Component
'use client';

import { useState } from 'react';

interface Props {
  readonly initialValue: string;
}

export default function Component({ initialValue }: Props) {
  const [value, setValue] = useState(initialValue);
  
  return (
    <div>
      <input 
        value={value} 
        onChange={(e) => setValue(e.target.value)} 
      />
    </div>
  );
}
```

### 2. API Routeパターン (Hono)
```typescript
import { Hono } from 'hono';
import { z } from 'zod';

const app = new Hono();

// バリデーションスキーマ
const CreateUserSchema = z.object({
  name: z.string().min(1),
  email: z.string().email(),
});

// エンドポイント
app.post('/users', async (c) => {
  try {
    const body = await c.req.json();
    const validated = CreateUserSchema.parse(body);
    
    // ビジネスロジック実行
    const user = await userService.create(validated);
    
    return c.json({ user }, 201);
  } catch (error) {
    if (error instanceof z.ZodError) {
      return c.json({ error: 'Validation failed', details: error.errors }, 400);
    }
    return c.json({ error: 'Internal server error' }, 500);
  }
});

export default app;
```

### 3. Drizzle ORM パターン
```typescript
// スキーマ定義
import { pgTable, serial, text, timestamp } from 'drizzle-orm/pg-core';

export const users = pgTable('users', {
  id: serial('id').primaryKey(),
  name: text('name').notNull(),
  email: text('email').notNull().unique(),
  createdAt: timestamp('created_at').defaultNow().notNull(),
});

// リポジトリパターン
interface UserRepository {
  create(data: CreateUserInput): Promise<User>;
  findById(id: number): Promise<User | null>;
  findByEmail(email: string): Promise<User | null>;
}

class DrizzleUserRepository implements UserRepository {
  constructor(private db: Database) {}

  async create(data: CreateUserInput): Promise<User> {
    const [user] = await this.db
      .insert(users)
      .values(data)
      .returning();
    return user;
  }

  async findById(id: number): Promise<User | null> {
    const [user] = await this.db
      .select()
      .from(users)
      .where(eq(users.id, id));
    return user || null;
  }
}
```

### 4. TanStack Query パターン
```typescript
// カスタムフック
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';

// 取得用
export function useUser(id: number) {
  return useQuery({
    queryKey: ['user', id],
    queryFn: () => userApi.getById(id),
    enabled: !!id,
  });
}

// 更新用
export function useUpdateUser() {
  const queryClient = useQueryClient();
  
  return useMutation({
    mutationFn: userApi.update,
    onSuccess: (updatedUser) => {
      // キャッシュ更新
      queryClient.setQueryData(['user', updatedUser.id], updatedUser);
      queryClient.invalidateQueries({ queryKey: ['users'] });
    },
  });
}
```

## エラーハンドリングパターン

### 1. Result型パターン
```typescript
type Result<T, E> = Ok<T> | Err<E>;

interface Ok<T> {
  readonly success: true;
  readonly data: T;
}

interface Err<E> {
  readonly success: false;
  readonly error: E;
}

// 使用例
function validateUser(input: unknown): Result<User, ValidationError> {
  if (!isValidUser(input)) {
    return { success: false, error: new ValidationError('Invalid user') };
  }
  return { success: true, data: input as User };
}
```

### 2. Domain Errorパターン
```typescript
abstract class DomainError extends Error {
  abstract readonly code: string;
}

class UserNotFoundError extends DomainError {
  readonly code = 'USER_NOT_FOUND';
  
  constructor(id: number) {
    super(`User with id ${id} not found`);
  }
}

class ValidationError extends DomainError {
  readonly code = 'VALIDATION_ERROR';
  
  constructor(message: string) {
    super(message);
  }
}
```

## テストパターン

### 1. 単体テストパターン
```typescript
import { describe, it, expect } from 'vitest';

describe('UserService', () => {
  it('should create user with valid input', async () => {
    // Arrange
    const input = { name: 'John', email: 'john@example.com' };
    const mockRepo = createMockUserRepository();
    const service = new UserService(mockRepo);

    // Act
    const result = await service.create(input);

    // Assert
    expect(result.success).toBe(true);
    if (result.success) {
      expect(result.data.name).toBe(input.name);
      expect(result.data.email).toBe(input.email);
    }
  });

  it('should return error with invalid input', async () => {
    // Arrange
    const input = { name: '', email: 'invalid-email' };
    const service = new UserService(mockRepo);

    // Act
    const result = await service.create(input);

    // Assert
    expect(result.success).toBe(false);
    if (!result.success) {
      expect(result.error).toBeInstanceOf(ValidationError);
    }
  });
});
```

### 2. 統合テストパターン
```typescript
import { describe, it, expect, beforeEach } from 'vitest';
import { testClient } from 'hono/testing';

describe('User API', () => {
  beforeEach(async () => {
    await resetTestDatabase();
  });

  it('POST /users should create user', async () => {
    // Arrange
    const client = testClient(app);
    const userData = { name: 'John', email: 'john@example.com' };

    // Act
    const response = await client.users.$post({ json: userData });

    // Assert
    expect(response.status).toBe(201);
    const result = await response.json();
    expect(result.user.name).toBe(userData.name);
  });
});
```

## セキュリティパターン

### 1. 入力検証パターン
```typescript
import { z } from 'zod';

// 厳密なスキーマ定義
const UserInputSchema = z.object({
  name: z.string().min(1).max(100).trim(),
  email: z.string().email().toLowerCase(),
  age: z.number().int().min(0).max(150),
});

// 使用例
function validateUserInput(input: unknown) {
  try {
    return UserInputSchema.parse(input);
  } catch (error) {
    throw new ValidationError('Invalid user input');
  }
}
```

### 2. 認証パターン
```typescript
import { verify } from 'hono/jwt';

// JWT検証ミドルウェア
const authMiddleware = async (c: Context, next: Next) => {
  const token = c.req.header('Authorization')?.replace('Bearer ', '');
  
  if (!token) {
    return c.json({ error: 'Authorization required' }, 401);
  }

  try {
    const payload = await verify(token, process.env.JWT_SECRET!);
    c.set('user', payload);
    await next();
  } catch {
    return c.json({ error: 'Invalid token' }, 401);
  }
};
```

## パフォーマンスパターン

### 1. キャッシングパターン
```typescript
// React Query での最適化
export function useUsers() {
  return useQuery({
    queryKey: ['users'],
    queryFn: userApi.getAll,
    staleTime: 5 * 60 * 1000, // 5分間キャッシュ
    cacheTime: 10 * 60 * 1000, // 10分間保持
  });
}

// Next.js での静的生成
export async function generateStaticParams() {
  const users = await userApi.getAll();
  return users.map(user => ({ id: user.id.toString() }));
}
```

### 2. データベース最適化パターン
```typescript
// N+1問題の回避
const usersWithPosts = await db
  .select()
  .from(users)
  .leftJoin(posts, eq(users.id, posts.userId));

// インデックス活用
const usersByEmail = await db
  .select()
  .from(users)
  .where(eq(users.email, email)) // emailにインデックス必要
  .limit(1);
```