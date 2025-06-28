# プロジェクト技術知見

## アーキテクチャ設計

### Clean Architecture構造
```
Domain Layer (最内層)
├── Entities: ビジネスルールとデータ
├── Value Objects: 不変オブジェクト
└── Repository Interface: データアクセス抽象化

Application Layer
├── Use Cases: ビジネスロジック調整
├── Services: アプリケーションサービス
└── DTOs: データ転送オブジェクト

Infrastructure Layer
├── Database: Drizzle ORM実装
├── External APIs: Hono API統合
└── Configurations: 設定管理

Presentation Layer
├── Next.js Pages/Components
├── API Routes: Hono統合
└── UI Components: Tailwind CSS
```

## 技術スタック知見

### Next.js 15 + TypeScript
- **App Router**: 新しいファイルベースルーティング
- **Server Components**: デフォルトでサーバーサイドレンダリング
- **Client Components**: 'use client'ディレクティブで明示
- **型安全性**: strict mode必須、any型禁止

### Drizzle ORM
- **スキーマファースト**: TypeScript型安全性重視
- **マイグレーション**: drizzle-kit使用
- **クエリビルダー**: SQL的な記述で直感的
- **リレーション**: 明示的なリレーション定義

### Hono API
- **軽量**: Express代替として高パフォーマンス
- **型安全**: TypeScript統合優秀
- **ミドルウェア**: 柔軟な認証・CORS対応
- **Validator**: Zod統合でリクエスト検証

### TanStack Query
- **キャッシュ管理**: サーバー状態の効率的管理
- **背景更新**: 自動的なデータ同期
- **楽観的更新**: UX向上のための先行更新
- **エラーハンドリング**: 統一されたエラー処理

## 開発ベストプラクティス

### コード品質
```typescript
// 良い例: 型安全性とバリデーション
interface UserCreateInput {
  readonly name: string;
  readonly email: string;
}

const createUser = (input: UserCreateInput): Result<User, ValidationError> => {
  // バリデーション必須
  if (!isValidEmail(input.email)) {
    return Err(new ValidationError('Invalid email'));
  }
  // 実装
  return Ok(user);
};

// 悪い例: any型使用
const createUser = (input: any) => {
  // バリデーションなし、型安全性なし
};
```

### エラーハンドリング
- **Result型**: 成功/失敗の明示的表現
- **カスタムエラー**: ドメイン固有エラークラス
- **境界処理**: Error Boundaryでの例外処理
- **ログ記録**: 構造化ロギング実装

### テスト戦略
- **単体テスト**: Domain層100%カバレッジ
- **統合テスト**: API エンドポイント検証
- **E2Eテスト**: 重要ユーザーフロー
- **型テスト**: TypeScriptコンパイラ活用

### セキュリティ
- **環境変数**: 機密情報の適切な管理
- **認証**: JWT/Session-based認証
- **認可**: ロールベースアクセス制御
- **入力検証**: Zodスキーマでの厳密検証

## パフォーマンス最適化

### Next.js最適化
- **Image Optimization**: next/imageコンポーネント使用
- **Code Splitting**: 動的インポート活用
- **Static Generation**: 可能な限りSSG採用
- **Caching**: ISRとEdge Cachingの活用

### データベース最適化
- **インデックス**: 適切なインデックス設計
- **N+1問題**: 事前ロード戦略
- **コネクションプール**: 効率的な接続管理
- **クエリ最適化**: EXPLAIN PLANでの分析

## CI/CD と開発環境

### 開発ツール
- **Biome**: 高速リンター・フォーマッター
- **TypeScript**: 厳密型チェック
- **Husky**: Git hooks管理
- **lint-staged**: コミット前検証

### デプロイメント
- **Vercel**: Next.js最適化プラットフォーム
- **環境分離**: dev/staging/prod環境
- **自動テスト**: CI/CDパイプライン統合
- **ロールバック**: 迅速な復旧戦略

## 学習と改善

### コードレビュー観点
1. **型安全性**: any型の排除
2. **可読性**: 意図の明確な表現
3. **テストカバレッジ**: 重要パスの検証
4. **セキュリティ**: 脆弱性の事前防止
5. **パフォーマンス**: ボトルネック特定

### リファクタリング指針
- **小さな変更**: 段階的改善
- **テスト先行**: 既存機能保護
- **ドキュメント更新**: 知識の共有
- **メトリクス監視**: 改善効果測定