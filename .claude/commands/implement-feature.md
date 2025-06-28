# Implement Feature

新機能を段階的に実装します。

## 使用法
```
/implement-feature [feature_description]
```

## プロンプト
以下の機能を実装してください：

**機能**: ${ARGUMENTS}

### Explore-Plan-Code-Commit パターンで実行

#### 1. Research Phase
- 既存のコードベース構造を理解
- 関連する既存機能を調査
- 使用可能なライブラリ・パターンを確認
- 類似の実装例を検索

#### 2. Planning Phase
- TodoWriteで実装計画を作成
- 必要なファイル・変更を特定
- テスト戦略を策定
- 段階的な実装ステップを定義

#### 3. Implementation Phase
- 各タスクをin_progressでマーク
- TDD（テスト駆動開発）アプローチを採用
- 既存のコード規約・パターンに従う
- 段階的に機能を実装
- 各ステップで動作確認

#### 4. Quality Assurance
- テスト実行（単体・統合）
- lint/typecheckコマンド実行
- セキュリティベストプラクティス確認
- パフォーマンス影響を検証

#### 5. Documentation
- 実装ドキュメントを作成（必要に応じて）
- コメント追加（明示的要求時のみ）
- README更新（必要に応じて）

全プロセスでTodoWriteを使用して進捗を追跡し、透明性を保ってください。