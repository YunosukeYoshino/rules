## コンポーネント設計と実装の規約

### 1. ディレクトリ構造とファイル配置

- アプリケーションの構造は App Router 形式（/app ディレクトリ）に準拠
- コンポーネントは /components ディレクトリに配置
  - UI コンポーネントは /components/ui に配置
  - 機能別コンポーネントは /components/[feature] に配置
- 複数のコンポーネントで共有されるフックは /hooks ディレクトリに配置
- ユーティリティ関数は /lib ディレクトリに配置
- ドメインモデルは /domain ディレクトリに配置
- API は /api ディレクトリに配置（Hono）
- Drizzle スキーマは /db/schema ディレクトリに配置
- 型定義は /types ディレクトリに配置

### 2. コンポーネントの実装

- 関数コンポーネントのみを使用
- Hooks を活用した状態管理
- Props は型定義を必ず指定し、required/default を明示
- Server Components と Client Components の区別を明確にする
  - Client Components には 'use client' ディレクティブを先頭に配置
  - データ取得は Server Components で行う
  - インタラクティブな UI は Client Components で実装

### 3. UI/UXデザイン

- shadcn/ui のコンポーネントを優先的に使用し、一貫したデザインを維持
- Tailwind CSS を使用したカスタマイズ
- レスポンシブデザインを考慮したクラス設定
- アクセシビリティを考慮した aria 属性の付与
- トランジションやアニメーションは適度に活用

### 4. 国際化対応

- next-intl を使用したテキストの国際化
- 日付や数値のフォーマットは各言語に対応
- 言語切り替えに対応したレイアウト設計

### 5. コンポーネントの種類別規約

#### ボタン系
- shadcn/ui の Button コンポーネントをベースに実装
- クリックハンドラは handle[Action] の形式で命名
- disabled 状態の視覚的フィードバックを実装
- loading 状態の表現を統一
- バリアントの種類（primary, secondary, destructive 等）を適切に使用

#### モーダル系
- shadcn/ui の Dialog コンポーネントをベースに実装
- isOpen と onOpenChange プロパティで表示制御
- フォーカストラップの実装
- キーボード操作（Escape）対応

#### テーブル系
- shadcn/ui の Table コンポーネントをベースに実装
- ページネーションの実装
- ソート・フィルタ機能の統一的な実装
- 空の状態の表示を統一
- ローディング状態の表示

### 6. エラーハンドリング

- try-catch による適切なエラーハンドリング
- ユーザーフレンドリーなエラーメッセージの表示
- エラー状態のログ記録
- Next.js の Error Boundary を活用したエラー表示

### 7. テスト容易性

- テスト可能なコンポーネント設計
- 副作用の分離
- データ取得と表示ロジックの分離
