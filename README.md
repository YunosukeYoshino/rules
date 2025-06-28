# Claude Code ベストプラクティス & ノウハウリポジトリ

このリポジトリは、**Claude Code**を使用した効率的な開発のためのベストプラクティス、設定ファイル、ナレッジベースを集約したものです。2024-2025年の最新Claude Codeベストプラクティスを完全統合し、生産性向上を実現します。

## 🎯 プロジェクト概要

**目的**: AI Rules Repository - Claude Code特化の開発環境最適化
**技術スタック**: Next.js 15 + TypeScript + Drizzle ORM + Hono + TanStack Query + Tailwind CSS
**生産性目標**: 45分タスク → 1パス完了の効率化実現

## 📂 リポジトリ構造

```mermaid
graph TD
    A[Claude Code Repository] --> B[".claude/"]
    B --> C["commands/ (カスタムコマンド)"]
    B --> D["docs/ (技術ドキュメント)"]
    B --> E["common-patterns.md"]
    B --> F["context.md"]
    B --> G["project-knowledge.md"]

    A --> H["CLAUDE.md (メインルール)"]
    A --> I["README.md (本ファイル)"]

    C --> C1["/gemini-search"]
    C --> C2["/analyze-structure"]
    C --> C3["/debug-workflow"]
    C --> C4["/implement-feature"]
    C --> C5["/refactor-code"]

    D --> D1["global/ (グローバル規約)"]
    D --> D2["backend/ (バックエンド技術)"]
    D --> D3["frontend/ (フロントエンド技術)"]
    D --> D4["structure/ (アーキテクチャ)"]
    D --> D5["testing/ (テスト戦略)"]

    style A fill:#e1f5fe,stroke:#01579b,stroke-width:3px
    style B fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    style H fill:#e8f5e8,stroke:#1b5e20,stroke-width:2px
```

## 🚀 Claude Code 統合ワークフロー

### Explore-Plan-Code-Commit パターン

```mermaid
flowchart LR
    A[🔍 Explore] --> B[📋 Plan]
    B --> C[⚡ Code]
    C --> D[📝 Commit]

    A --> A1[コードベース理解<br/>アーキテクチャ把握]
    B --> B1[ultrathink使用<br/>詳細計画立案]
    C --> C1[TDD実装<br/>段階的検証]
    D --> D1[適切なコミット<br/>品質確保]

    style A fill:#e3f2fd,stroke:#01579b
    style B fill:#fff3e0,stroke:#e65100
    style C fill:#e8f5e8,stroke:#2e7d32
    style D fill:#fce4ec,stroke:#ad1457
```

## 🎯 核となる最適化ポイント

### 1. **コンテキスト管理戦略**
- **`/clear`**: 新タスク開始時の完全コンテキストリセット
- **`/compact`**: 自然な区切りでのコンテキスト要約（手動推奨）
- **要約戦略**: 5Kトークン程度の事前マークダウン仕様書作成
- **チャンキング**: ディレクトリ単位での段階的作業分割

### 2. **メモリ階層活用**
```mermaid
graph TD
    A[メモリ階層構造] --> B[CLAUDE.md<br/>常時読み込み]
    A --> C[".claude/docs/<br/>オンデマンド参照"]
    A --> D[".claude/commands/<br/>自動化ワークフロー"]

    B --> B1["@README.md<br/>プロジェクト概要"]
    B --> B2["@package.json<br/>スクリプト確認"]

    C --> C1["@.claude/docs/global/*<br/>グローバル規約"]
    C --> C2["@.claude/docs/structure/*<br/>アーキテクチャ"]

    D --> D1["/review_pr<br/>コードレビュー"]
    D --> D2["/debug_session<br/>デバッグループ"]
```

### 3. **TDD最適化パターン**
1. **テスト先行指示**: 「TDDで進める」を明示してモック実装回避
2. **失敗確認**: テスト実行で期待通りの失敗を確認
3. **実装分離**: テスト修正禁止を徹底して設計保持
4. **段階的成功**: 全テスト通過まで反復的実装継続
5. **AIとTDDの親和性**: 幻覚対策とスコープドリフト防止に最適

## 🛠️ カスタムコマンド活用

`.claude/commands/`ディレクトリ内のカスタムコマンド例：

| コマンド | 用途 | 効果 |
|---------|------|------|
| `/review_pr` | PR向け体系的コードレビュー | 品質保証の自動化 |
| `/debug_session` | 構造化デバッグループ | 問題解決の効率化 |
| `/security_audit` | セキュリティ脆弱性スキャン | セキュリティ品質向上 |
| `/test_coverage` | テストカバレッジ分析 | テスト品質可視化 |
| `/performance_profile` | パフォーマンス分析 | 最適化提案生成 |

## 📈 パフォーマンス監視指標

- **生産性目標**: 45分タスク → 1パス完了の効率化実現
- **品質メトリクス**: テスト通過率100%・コード規約違反ゼロ維持
- **コスト効率化**: トークン消費監視・並列ツール実行活用
- **自動化率向上**: カスタムコマンドによる反復作業削減

## 🔧 技術スタック統合

```mermaid
graph LR
    A[Next.js 15] --> B[TypeScript]
    B --> C[Drizzle ORM]
    C --> D[Hono API]
    D --> E[TanStack Query]
    E --> F[Tailwind CSS]

    A --> A1[App Router<br/>Server Components]
    C --> C1[Type-safe SQL<br/>Schema管理]
    D --> D1[軽量API<br/>エッジ対応]
    E --> E1[データフェッチ<br/>キャッシュ最適化]

    style A fill:#000000,color:#ffffff
    style B fill:#3178c6,color:#ffffff
    style C fill:#c5f74f,color:#000000
    style D fill:#e36002,color:#ffffff
    style E fill:#ff4154,color:#ffffff
    style F fill:#06b6d4,color:#ffffff
```

## 🎓 ベストプラクティス

### Claude Code公式ガイダンス
- [Anthropic公式ドキュメント](https://docs.anthropic.com/en/docs/claude-code)
- [GitHub: awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code)
- [TDD with Claude Code](https://thenewstack.io/claude-code-and-the-art-of-test-driven-development/)

### 日本語コミュニティ知見
- [Zenn: Claude Code ベストプラクティス](https://zenn.dev/farstep/articles/claude-code-best-practices)
- [Zenn: 私のマイCLAUDE.md解説](https://zenn.dev/dirtyman/articles/ddbec05fd9fbb4)
- [Zenn: Claude Code 逆引きコマンド事典](https://zenn.dev/ml_bear/articles/84e92429698177)

## 🚦 クイックスタート

1. **リポジトリクローン**
```bash
git clone https://github.com/YunosukeYoshino/rules.git
cd rules
```

2. **CLAUDE.mdの確認**
```bash
cat CLAUDE.md  # メインルールファイル確認
```

3. **カスタムコマンドの活用**
```bash
# Claude Code起動後
/gemini-search "検索クエリ"  # Gemini検索
/analyze-structure           # プロジェクト構造分析
/implement-feature          # 機能実装ワークフロー
```

4. **プロジェクトへの適用**
```bash
# 自分のプロジェクトにCLAUDE.mdをコピー
cp CLAUDE.md /path/to/your/project/
cp -r .claude /path/to/your/project/
```

## 🤝 貢献ガイドライン

### 新しいベストプラクティス追加
1. `.claude/docs/`内の適切なカテゴリに配置
2. `CLAUDE.md`で統合ポイントを参照
3. 実証されたワークフローのみを含める
4. 日本語・英語併記を維持

### カスタムコマンド開発
1. `.claude/commands/`に新規Markdownファイル作成
2. `$ARGUMENTS`キーワードでパラメータ化
3. 再利用可能な形式で設計
4. 使用例とドキュメントを含める

## 📝 ライセンス

MIT License - 自由に使用、修正、配布可能

---

**🎯 目標**: Claude Code の力を最大限に活用し、開発生産性を革新的に向上させる
