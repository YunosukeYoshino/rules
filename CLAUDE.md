# Claude Code 開発ルール | Development Rules

## プロジェクト概要 | Project Overview
**目的**: AI Rules Repository - 各開発環境に特化したAIアシスタンス最適化  
**Purpose**: AI Rules Repository - Optimizing AI assistance for specific development environments  

**技術スタック | Tech Stack**: Next.js 15 + TypeScript + Drizzle ORM + Hono + TanStack Query + Tailwind CSS

## コマンド集 | Common Commands
```bash
# ビルド | Build
npm run build

# テスト | Test  
npm test

# リント・型チェック | Lint & Type Check
npm run lint
npm run typecheck

# 開発サーバー | Dev Server
npm run dev
```

## 統合ドキュメント | Integrated Documentation
参照ファイル | Reference Files:
- .claude/* (Claude Code専用設定)
- docs/* (全技術ドキュメント)

## 基本原則

### 1. タスク駆動開発
- 複雑で複数ステップのタスクにはTodoWriteツールを使用
- 大きな機能を管理可能なステップに分解
- 作業開始前にタスクをin_progressにマーク
- 完了後すぐにタスクを完了状態にする
- todoリストを通じて進捗の透明性を維持

### 2. 防御的セキュリティフォーカス
- 防御的セキュリティタスクのみ支援
- 悪意のあるコードの作成、修正、改良を拒否
- セキュリティ分析、検知ルール、脆弱性説明は許可
- 防御ツールとセキュリティドキュメントをサポート

### 3. コード品質と規約
- 既存のコードパターンと規約に従う
- 使用前にライブラリの可用性を確認
- 一貫したコードスタイルを維持
- 明示的に要求されない限りコメントを追加しない
- 新しいファイル作成よりも既存ファイルの編集を優先

### 4. 検索と分析
- コードベース全体のキーワード検索にはTaskツールを使用
- 特定のファイルパターンにはGlob/Grepを使用
- コンテキストのために近隣ファイルを調査
- 変更前にプロジェクト構造を理解

## 技術スタック優先順位
docs/global/technical_stack.mdに基づく:
- TypeScriptを使用したNext.js
- データベース操作にDrizzle ORM
- API開発にHono
- データ取得にTanStack Query
- スタイリングにTailwind CSS

## 開発ワークフロー | Development Workflow

### Explore-Plan-Code-Commit パターン
1. **Research Phase**: ファイル読み取り、アーキテクチャ理解
2. **Planning Phase**: 構造化された実装計画作成  
3. **Implementation Phase**: 段階的コーディングと検証
4. **Commit Phase**: 適切な帰属を含む明確なコミット

### 初期分析 | Initial Analysis
1. "@" シンタックスでファイル・ディレクトリ参照
2. 広範囲な設計質問から開始、その後詳細化
3. 検索ツールでコードベース構造理解
4. 既存パターンと規約確認
5. 複雑タスク用のtodoリスト作成

### 実装 | Implementation  
1. 関連todoをin_progressにマーク
2. 既存コードパターンに従う
3. テスト駆動開発(TDD)の活用
4. 段階的ソリューション実装
5. 利用可能テストで変更検証
6. todoを即座に完了状態にマーク

### TDD最適化パターン | TDD Optimization Patterns
1. **テスト先行指示**: 「TDDで進める」を明示してモック実装回避
2. **失敗確認**: テスト実行で期待通りの失敗を確認
3. **実装分離**: テスト修正禁止を徹底して設計保持
4. **段階的成功**: 全テスト通過まで反復的実装継続
5. **品質保証**: テストカバレッジと型安全性の両立確保
6. **AIとTDDの親和性**: 幻覚対策とスコープドリフト防止に最適

### 品質保証 | Quality Assurance
1. lint/typecheckコマンド実行必須
2. セキュリティベストプラクティス遵守
3. シークレット露出防止確認
4. プロジェクト規約準拠検証
5. テストカバレッジ確保

### パフォーマンス監視 | Performance Monitoring
- **生産性目標**: 45分タスク→1パス完了の効率化実現
- **品質メトリクス**: テスト通過率100%・コード規約違反ゼロ維持
- **コスト効率化**: トークン消費監視・並列ツール実行活用
- **セッション管理**: 適切な`/clear`・`/compact`使用でパフォーマンス維持
- **自動化率向上**: カスタムコマンドによる反復作業削減

## AI駆動ルール統合
docs/global/ai_driven_rules.mdに従い:
- 重要な機能の実装ドキュメントを作成
- ドキュメントを`/docs/implementations/`ディレクトリに配置
- アーキテクチャ、設計パターン、使用例を含める
- AI生成ソリューションの追跡可能性を維持

## ファイル操作
- 常に絶対パスを使用
- 編集前にファイルを読み取り
- 同じファイルへの複数変更にはMultiEditを優先
- ファイル操作に適切なエラーハンドリングを使用

## コミュニケーションスタイル
- 簡潔な回答（詳細が要求されない限り最大4行）
- 不要な前置きなしの直接的な答え
- CLI表示にマークダウン形式を使用
- 実行前に重要でないbashコマンドを説明

## Git操作
- 明示的に要求されない限りコミットしない
- 適切な帰属を含むコミットメッセージ形式を使用
- git status、diff、logコマンドを並列実行
- リポジトリのコミットメッセージ規約に従う

## パーミッション管理 | Permission Management
- 保守的なシステム変更アプローチ
- `/permissions` コマンドでツールアクセス管理
- セッション固有の `--allowedTools` フラグ活用
- 隔離環境外では `--dangerously-skip-permissions` 回避

## カスタムコマンド | Custom Commands
`.claude/commands/` ディレクトリでスラッシュコマンド管理:
- Markdownファイルとしてプロンプトテンプレート保存
- `$ARGUMENTS` キーワードでパラメータ化
- 反復ワークフロー（デバッグ、ログ解析等）に活用

### 推奨カスタムコマンド例
- `/review_pr`: PR向け体系的コードレビュー
- `/debug_session`: 構造化デバッグループワークフロー
- `/refactor_check`: リファクタリング前後の整合性確認
- `/security_audit`: セキュリティ脆弱性スキャンと報告
- `/test_coverage`: テストカバレッジ分析と改善提案
- `/performance_profile`: パフォーマンス分析と最適化提案

## 統合ポイント | Integration Points
- **コーディング規約**: `@.claude/docs/global/coding_conventions.md`で統一基準
- **コミット規約**: `@.claude/docs/global/commit_message_conventions.md`で履歴管理
- **セキュリティ**: `@.claude/docs/global/security.md`で防御的実践
- **プロジェクト構成**: `@.claude/docs/structure/*`でアーキテクチャ指針
- **技術スタック**: `@.claude/docs/global/technical_stack.md`で技術選択基準
- **統合例**: Next.js + Drizzle + Hono エコシステム最適化

## コンテキスト管理 | Context Management

### セッション最適化
- `/clear`: 新タスク開始時の完全コンテキストリセット
- `/compact`: 自然な区切りでのコンテキスト要約（手動コンパクト推奨）
- コンテキスト残量監視：自動コンパクト前の戦略的手動実行
- 長時間セッション対策：無関係情報蓄積によるパフォーマンス低下防止

### 大規模コードベース戦略
- **要約戦略**: 5Kトークン程度の事前マークダウン仕様書作成
- **チャンキング**: ディレクトリ単位での段階的作業分割
- **agentic検索**: 自動プロジェクト構造把握とコンテキスト最適化
- **トークン最適化**: 必要最小限のコンテキスト維持でコスト効率化

## メモリ階層活用 | Memory Hierarchy Optimization

### ファイル参照最適化
- プロジェクト概要：`@README.md`で基本情報参照
- 利用可能コマンド：`@package.json`でスクリプト確認
- アーキテクチャ：`@.claude/docs/structure/*`で構造理解
- 技術スタック：`@.claude/docs/global/technical_stack.md`で技術選択指針

### 情報分離戦略
- **CLAUDE.md**: 常時必要な基本情報とルール
- **docs/**: オンデマンド参照資料（アーキテクチャ、規約等）
- **.claude/commands/**: 自動化ワークフローとカスタムコマンド
- **階層的読み込み**: サブディレクトリのCLAUDE.mdも再帰的活用

## 高度な機能 | Advanced Features  
- **Resume機能**: `--continue` や `--resume` で会話継続
- **Extended Thinking**: 複雑な設計判断に「ultrathink」で深い思考モード発動
- **Git Worktrees**: 並列Claude Code セッション実行
- **Visual Iteration**: スクリーンショット→実装→結果確認のサイクル
- **Multi-modal機能**: UIモックアップ解析によるHTML/CSS自動生成