## ユニットテスト実装の規約

### 1. コンポーネントテスト

#### ファイル構成
- テストファイルの命名規則: `[ComponentName].test.tsx`
- テストファイルの配置: `/tests/unit/components/`
- テストケースは機能単位でグループ化

#### テストケース設計
- コンポーネントのレンダリング状態の検証
- Props の動作確認
- ユーザーインタラクションのテスト
- 非同期操作の検証
- エラー状態のハンドリング

```typescript
// tests/unit/components/Button.test.tsx
import { test, expect, describe } from 'bun:test';
import { render, fireEvent } from '@testing-library/react';
import { Button } from '@/components/ui/Button';

describe('Button', () => {
  test('renders with default props', () => {
    const { getByRole } = render(<Button>Click me</Button>);
    const button = getByRole('button');

    expect(button).toBeDefined();
    expect(button.textContent).toBe('Click me');
  });

  test('calls onClick when clicked', () => {
    const handleClick = jest.fn();
    const { getByRole } = render(
      <Button onClick={handleClick}>Click me</Button>
    );

    fireEvent.click(getByRole('button'));
    expect(handleClick).toHaveBeenCalledTimes(1);
  });

  test('is disabled when disabled prop is true', () => {
    const { getByRole } = render(<Button disabled>Click me</Button>);
    const button = getByRole('button');

    expect(button).toHaveAttribute('disabled');
  });
});
```

### 2. ドメインロジックテスト

#### ファイル構成
- テストファイルの命名規則: `[DomainClass].test.ts`
- テストファイルの配置: `/tests/unit/domain/`
- ビジネスルールごとにグループ化

```typescript
// tests/unit/domain/User.test.ts
import { test, expect, describe } from 'bun:test';
import { User } from '@/domain/User';

describe('User Domain Entity', () => {
  test('creates valid user', () => {
    const user = new User({
      name: 'John Doe',
      email: 'john@example.com',
    });

    expect(user.name).toBe('John Doe');
    expect(user.email).toBe('john@example.com');
  });

  test('validates email format', () => {
    expect(() => {
      new User({
        name: 'John Doe',
        email: 'invalid-email',
      });
    }).toThrow('Invalid email format');
  });
});
```

### 3. リポジトリテスト

#### ファイル構成
- テストファイルの命名規則: `[RepositoryName].test.ts`
- テストファイルの配置: `/tests/unit/repositories/`
- CRUD 操作ごとにグループ化

```typescript
// tests/unit/repositories/UserRepository.test.ts
import { test, expect, describe, beforeEach, afterEach } from 'bun:test';
import { D1UserRepository } from '@/infrastructure/repositories/D1UserRepository';
import { mockDb } from '@/tests/mocks/db';

describe('D1UserRepository', () => {
  let repository: D1UserRepository;

  beforeEach(() => {
    repository = new D1UserRepository(mockDb);
  });

  afterEach(() => {
    mockDb.reset();
  });

  test('findById returns user when exists', async () => {
    const userId = '123';
    mockDb.prepare('users', [
      { id: userId, name: 'Test User', email: 'test@example.com' }
    ]);

    const user = await repository.findById(userId);

    expect(user).not.toBeNull();
    expect(user?.id).toBe(userId);
    expect(user?.name).toBe('Test User');
  });

  test('findById returns null when user does not exist', async () => {
    const user = await repository.findById('non-existent');
    expect(user).toBeNull();
  });
});
```

### 4. API テスト

#### ファイル構成
- テストファイルの命名規則: `[Route].test.ts`
- テストファイルの配置: `/tests/unit/api/`
- エンドポイントごとにグループ化

```typescript
// tests/unit/api/userRoutes.test.ts
import { test, expect, describe } from 'bun:test';
import { usersRoutes } from '@/api/users';
import { Hono } from 'hono';

describe('User API Routes', () => {
  test('GET /users/:id returns 200 with valid user', async () => {
    // セットアップ...

    const app = new Hono().route('/users', usersRoutes);
    const res = await app.request('/users/123');

    expect(res.status).toBe(200);

    const body = await res.json();
    expect(body.data.id).toBe('123');
  });

  test('GET /users/:id returns 404 when user not found', async () => {
    // セットアップ...

    const app = new Hono().route('/users', usersRoutes);
    const res = await app.request('/users/nonexistent');

    expect(res.status).toBe(404);
  });
});
