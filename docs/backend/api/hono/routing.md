## Hono ルーティング規約 (TypeScript)

### 1. リソースベースルーティング

-   HTTP メソッドをリソースのアクションにマッピングするには、リソースベースのルーティングを使用します。
-   `GET`、`POST`、`PUT`、`DELETE` などの一般的な操作のルートを定義します。

### 2. ルートグループ化

-   `route` メソッドを使用して、関連するルートを共通のプレフィックスの下にグループ化します。
-   これにより、組織が改善され、コードの重複が削減されます。

```typescript
import { Hono } from 'hono';

const api = new Hono();

const usersRoutes = new Hono();

usersRoutes.get('/', (c) => {
    return c.json({ message: 'すべてのユーザーを取得' });
});

usersRoutes.post('/', (c) => {
    return c.json({ message: '新しいユーザーを作成' });
});

api.route('/users', usersRoutes);
```

### 3. 動的ルート

-   パラメーターを使用して、さまざまなリソース ID を処理できる動的ルートを作成します。
-   `c.req.param` メソッドを使用してパラメーターにアクセスします。

```typescript
usersRoutes.get('/:id', (c) => {
    const id = c.req.param('id');
    return c.json({ message: `ユーザー ID ${id} を取得` });
});
```

### 4. バージョニング

-   バージョニングを使用して、API の変更を時間の経過と共に管理します。
-   URL プレフィックス (例: `/v1/users`) を使用してバージョニングを実装します。

### 5. ネスト

-   リソースをネストして、それらの間の関係を表します (例: `/users/:userId/posts`)。

```typescript
import { Hono } from 'hono';

const api = new Hono();

const usersRoutes = new Hono();
const postsRoutes = new Hono();

postsRoutes.get('/', (c) => {
    const userId = c.req.param('userId');
    return c.json({ message: `ユーザー ${userId} のすべての投稿を取得` });
});

usersRoutes.route('/:userId/posts', postsRoutes);

api.route('/users', usersRoutes);
