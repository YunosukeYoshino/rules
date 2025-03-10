# AI ルールリポジトリ

このリポジトリは、様々なAIアシスタント用のルールセットを集めたものです。各開発環境やフレームワークに特化したAIアシスタンスを最適化することを目的としています。

## リポジトリ構造

```mermaid
graph TD
    A[AI Rules Repository] --> B[windsurf]
    B --> C[shopify]
    C --> D[.windsurfrules]
    A --> E[future directories]
    E --> F[Cline Rules]
    E --> G[Other AI Rules]

    style A fill:#f9f,stroke:#333,stroke-width:2px
    style E fill:#bbf,stroke:#333,stroke-width:2px
    style D fill:#bfb,stroke:#333,stroke-width:2px
```

## ルールセット構造

```mermaid
mindmap
    root((AIルール構造))
        基本動作原則
            指示の理解
            分析とプランニング
            実装と検証
            フィードバック
        技術要件
            コア技術
            フレームワーク
            開発ツール
        品質管理
            コード品質
            パフォーマンス
            セキュリティ
            UI/UX
        実装プロセス
            分析フェーズ
            実装フェーズ
            検証フェーズ
            最終確認
```

## 現在のルールセット

### Windsurf Shopifyルール
`windsurf/shopify/.windsurfrules` に配置

```mermaid
graph LR
    A[Shopifyルール] --> B[技術要件]
    A --> C[開発プロセス]
    A --> D[品質基準]

    B --> B1[Liquid]
    B --> B2[JavaScript]
    B --> B3[SCSS]

    C --> C1[テンプレート]
    C --> C2[セクション]
    C --> C3[スニペット]

    D --> D1[パフォーマンス]
    D --> D2[アクセシビリティ]
    D --> D3[最適化]
```

## 拡張計画

```mermaid
timeline
    title 拡張ロードマップ
    section 現在
        Windsurf Shopifyルール
    section Phase 1
        Clineルール : 高度な開発支援
    section Phase 2
        フレームワーク固有ルール : Next.js/React/Vue
    section Phase 3
        特殊環境ルール : Cloud/Mobile/AI
```

## ルールセットの実装プロトコル

```mermaid
flowchart TD
    A[新規ルールセット提案] --> B{要件分析}
    B --> C[技術スタック定義]
    C --> D[プロトコル設計]
    D --> E[実装ガイドライン]
    E --> F[テストと検証]
    F --> G{承認}
    G --> |Yes| H[リポジトリ統合]
    G --> |No| D
```

## 貢献ガイドライン

新しいルールセットを追加する際は：
1. 既存のディレクトリ構造に従う
2. 包括的なドキュメントを含める
3. 一貫した形式を維持する
4. 複数のAI間での互換性を考慮する
5. AI固有のベストプラクティスに従う

