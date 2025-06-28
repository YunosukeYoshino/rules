# プロジェクトコンテキスト

## プロジェクト概要
- **名前**: AI Rules Repository
- **目的**: 様々なAIアシスタント用のルールセットを集約し、各開発環境やフレームワークに特化したAIアシスタンスを最適化する
- **現在の状況**: Claude Code専用ルールセットに統合完了

## 技術スタック
- **フレームワーク**: Next.js 15 (App Router)
- **言語**: TypeScript 5 (strict mode)
- **スタイリング**: Tailwind CSS
- **ORM**: Drizzle ORM
- **API**: Hono
- **データ取得**: TanStack Query
- **パッケージマネージャー**: npm/yarn
- **アーキテクチャ**: Clean Architecture + DDD

## 開発原則
- **防御的セキュリティ**: 悪意のあるコードの作成・改良を拒否
- **タスク駆動開発**: TodoWriteツールを活用した透明性の高い開発
- **品質重視**: 
  - strict TypeScript
  - テストカバレッジ重視
  - セキュリティベストプラクティス遵守
- **コード規約**: 既存パターンへの準拠、不要なコメント避ける

## プロジェクト構造
```
.claude/
├── context.md           # このファイル
├── project-knowledge.md # 技術知見とベストプラクティス
├── common-patterns.md   # 共通パターンとテンプレート
├── debug-log.md        # デバッグとトラブルシューティング
└── commands/           # よく使用するコマンド集
```

## 統合ポイント
- **メモリバンク**: memory-bank/* 全ファイル参照
- **ドキュメント**: docs/* 全ファイル参照
- **重要ファイル**:
  - `docs/global/technical_stack.md`
  - `docs/global/ai_driven_rules.md`
  - `docs/global/security.md`
  - `docs/global/coding_conventions.md`

## 品質メトリクス
- **テスト**: 利用可能なテストスイートでの検証必須
- **リント**: lint/typecheckコマンド実行必須
- **セキュリティ**: シークレット露出チェック
- **Git**: 明示的要求時のみコミット実行