## Hono ミドルウェア規約 (TypeScript)

### 1. 認証ミドルウェア

-   `Authorization` ヘッダーから JWT トークンを検証します。
-   後続のハンドラーで使用するために、コンテキストにユーザーオブジェクトを設定します。
-   トークンが無効または存在しない場合は、401 Unauthorized エラーを返します。

```typescript
import { verify } from 'hono/jwt';
import { MiddlewareHandler } from 'hono';

interface CustomEnv {
  Variables: {
    userId: string
  }
}

const authMiddleware: MiddlewareHandler<CustomEnv> = async (c, next) => {
    const token = c.req.header('Authorization')?.split(' ')[1];

    if (!token) {
        return c.json({ message: 'Unauthorized' }, 401);
    }

    try {
        const decoded = await verify(token, 'secret');
        c.set('userId', decoded.id);
        await next();
    } catch (error) {
        return c.json({ message: 'Unauthorized' }, 401);
    }
};
```

### 2. ロギングミドルウェア

-   受信リクエストを、関連情報 (メソッド、パス、IP アドレス) と共にログに記録します。
-   応答時間を測定してログに記録します。

```typescript
import { MiddlewareHandler } from 'hono';

const loggingMiddleware: MiddlewareHandler = async (c, next) => {
    const start = Date.now();
    await next();
    const end = Date.now();
    console.log(`${c.req.method} ${c.req.url} - ${end - start}ms`);
};
```

### 3. CORS ミドルウェア

-   特定のオリジンからのリクエストを許可するように、Cross-Origin Resource Sharing (CORS) を構成します。

```typescript
import { cors } from 'hono/cors';
import { MiddlewareHandler } from 'hono';

const corsMiddleware: MiddlewareHandler = cors({
    origin: '*', // すべてのオリジンを許可
    allowMethods: ['GET', 'POST', 'PUT', 'DELETE'],
    allowHeaders: ['Content-Type', 'Authorization'],
});
```

### 4. エラー処理ミドルウェア

-   後続のハンドラーによってスローされたエラーをキャッチします。
-   標準化されたエラー応答を返します。

```typescript
import { MiddlewareHandler } from 'hono';

const errorHandlingMiddleware: MiddlewareHandler = async (c, next) => {
    try {
        await next();
    } catch (error) {
        console.error(error);
        return c.json({ message: 'Internal Server Error' }, 500);
    }
};
