# AI ルールリポジトリ

このリポジトリは、様々なAIアシスタント用のルールセットを集めたものです。各開発環境やフレームワークに特化したAIアシスタンスを最適化することを目的としています。

## リポジトリ構造

```mermaid
graph TD
    A[AI Rules Repository] --> B[windsurf]
    B --> C[shopify]
    C --> D[.windsurfrules]
    B --> R[remix-cloudflare]
    R --> S[.windsurfrules]
    A --> E[cline]
    E --> F[.clinerules]
    A --> G[future directories]
    G --> H[Other AI Rules]

    style A fill:#f9f,stroke:#333,stroke-width:2px
    style B fill:#bbf,stroke:#333,stroke-width:2px
    style E fill:#bbf,stroke:#333,stroke-width:2px
    style D fill:#bfb,stroke:#333,stroke-width:2px
    style S fill:#bfb,stroke:#333,stroke-width:2px
    style F fill:#bfb,stroke:#333,stroke-width:2px
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

### Windsurf Remix Cloudflareルール
`windsurf/remix-cloudflare/.windsurfrules` に配置

```mermaid
graph LR
    A[Remix Cloudflareルール] --> B[技術スタック]
    A --> C[開発プロセス]
    A --> D[デプロイメント]

    B --> B1[Remix]
    B --> B2[TypeScript]
    B --> B3[Cloudflare Workers]
    B --> B4[D1 Database]
    
    C --> C1[ルーティング]
    C --> C2[データローダー]
    C --> C3[アクション]
    
    D --> D1[Wrangler]
    D --> D2[環境変数]
    D --> D3[CI/CD]
```

### Cline Next.js開発ルール
`cline/.clinerules` に配置

```mermaid
graph LR
    A[Next.js開発ルール] --> B[技術スタック]
    A --> C[開発環境設定]
    A --> D[AI駆動ルール]
    A --> E[セキュリティ]
    A --> F[コーディング規約]

    B --> B1[Next.js]
    B --> B2[TypeScript]
    B --> B3[Drizzle]
    B --> B4[TanStack Query]
    B --> B5[Hono]
    
    C --> C1[VSCode設定]
    C --> C2[Biome設定]
    
    D --> D1[実装ドキュメント]
    D --> D2[コード生成]
    D --> D3[知識共有]
    
    E --> E1[機密管理]
    E --> E2[API認証]
    
    F --> F1[型安全性]
    F --> F2[コンポーネント設計]
```

## 拡張計画

```mermaid
timeline
    title 拡張ロードマップ
    section 現在
        Windsurf Shopifyルール
        Windsurf Remix Cloudflareルール
        Cline Next.js開発ルール
    section Phase 1
        AI駆動開発ルール : コード生成最適化
    section Phase 2
        フレームワーク固有ルール : Vue/React Native
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

## AI アシスタント別の使用方法

### Cline
Cline AIアシスタントでは、`.clinerules` ファイルを直接参照して、特定の開発環境やフレームワークに最適化された支援を提供します。
- *使用方法*: プロンプトに「@cline rules/cline/.clinerules」を含めることで、Next.js開発用の設定を適用します。

### WindSurf
WindSurf AIアシスタントでは、`.windsurfrules` ファイルを使用して、特定の開発環境に特化した支援を受けられます。
- *Shopify開発*: プロンプト内で「@windsurf rules/windsurf/shopify/.windsurfrules」を指定します。
- *Remix Cloudflare開発*: プロンプト内で「@windsurf rules/windsurf/remix-cloudflare/.windsurfrules」を指定します。
