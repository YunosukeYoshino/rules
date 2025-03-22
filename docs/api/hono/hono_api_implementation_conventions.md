## Hono API 実装の規約

### 1. ファイル構成

- API ルートは /api ディレクトリに配置
- ルーティングはリソース単位でグループ化

```typescript
// api/index.ts
import { Hono } from 'hono';
import { usersRoutes } from './users';
import { postsRoutes } from './posts';

const api = new Hono();

api.route('/users', usersRoutes);
api.route('/posts', postsRoutes);

export { api };
```

```typescript
// api/users.ts
import { Hono } from 'hono';
import { zValidator } from '@hono/zod-validator';
import { z } from 'zod';
import { getUserUseCase } from '@/application/useCases/getUserUseCase';

const usersRoutes = new Hono();

const userSchema = z.object({
  name: z.string().min(1),
  email: z.string().email(),
});

usersRoutes.get('/:id', async (c) => {
  const id = c.req.param('id');

  try {
    const user = await getUserUseCase.execute(id);

    if (!user) {
      return c.json({ error: 'User not found' }, 404);
    }

    return c.json({ data: user });
  } catch (error) {
    return c.json({ error: 'Internal server error' }, 500);
  }
});

usersRoutes.post('/', zValidator('json', userSchema), async (c) => {
  const data = c.req.valid('json');

  try {
    const user = await createUserUseCase.execute(data);
    return c.json({ data: user }, 201);
  } catch (error) {
    if (error instanceof ValidationError) {
      return c.json({ error: error.message }, 400);
    }
    return c.json({ error: 'Internal server error' }, 500);
  }
});

export { usersRoutes };
```

### 2. リクエスト処理

- Zod を使用した入力バリデーション
- ミドルウェアを活用した共通処理（認証、ロギングなど）
- エラーハンドリングの統一

### 3. レスポンス形式

```typescript
// 成功時
return c.json({ data: result }, 200);

// エラー時
return c.json({ error: { message: "エラーメッセージ" } }, errorCode);
