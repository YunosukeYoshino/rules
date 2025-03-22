## Next.js ルーティング規約 (TypeScript)

### 1. ファイルベースルーティング

Next.js では、ファイルベースのルーティングシステムを採用しています。`app` ディレクトリ内の各ファイルがルートになります。

### 2. ルートセグメント

-   **フォルダ:** ルートセグメントを定義します。
-   **page.tsx:** ルートを公開します。
-   **layout.tsx:** ルートセグメントとその子で共有される UI を定義します。
-   **template.tsx:** レイアウトに似ていますが、ナビゲーション間で状態を保持しません。
-   **loading.tsx:** ローディング UI を表示します。
-   **not-found.tsx:** ルートが見つからない場合に表示されます。
-   **error.tsx:** エラー UI を表示します。
-   **route.ts:** サーバーエンドポイントを作成します。
-   **middleware.ts:** ルートがアクセスされる前にコードを実行します。

### 3. 動的ルート

角括弧を使用して、動的ルートセグメントを作成します (例: `[id]`)。

```typescript
// app/users/[id]/page.tsx
interface Props {
  params: { id: string };
}

export default function UserPage({ params }: Props) {
  const userId = params.id;
  return <div>User ID: {userId}</div>;
}
```

### 4. ルートグループ

括弧を使用して、URL 構造に影響を与えずにルートをグループ化します (例: `(group)`)。

### 5. パラレルルート

名前付きスロットを使用して、同じレイアウトに複数のページをレンダリングします (例: `@slot`)。

### 6. インターセプトルート

現在のレイアウト内で、アプリケーションの別の部分からルートをロードします。
