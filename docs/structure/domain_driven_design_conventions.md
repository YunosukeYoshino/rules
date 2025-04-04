## ドメイン駆動設計の規約

### 1. レイヤー構造

- ドメイン層
  - エンティティ
  - 値オブジェクト
  - ドメインサービス
  - リポジトリインターフェース
- アプリケーション層
  - ユースケース
  - コマンド/クエリハンドラ
- インフラストラクチャ層
  - リポジトリ実装
  - 外部サービスアダプター
- インターフェース層
  - API コントローラー
  - UI コンポーネント

### 2. ドメイン層の実装

- エンティティと値オブジェクトは不変性を保つ
- ビジネスルールはドメイン層に集約する
- 集約ルートを通じてのみエンティティにアクセス
- ドメインイベントを使用して副作用を分離

### 3. リポジトリパターン

- リポジトリインターフェースはドメイン層で定義
- 実装はインフラストラクチャ層で行う
- D1 と Drizzle を使用したリポジトリ実装
