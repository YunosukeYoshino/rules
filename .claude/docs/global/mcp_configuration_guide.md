# MCP設定ガイド | MCP Configuration Guide

## 概要 | Overview

Model Context Protocol (MCP) は、AI アシスタントが外部ツールやデータソースにアクセスするためのオープンプロトコルです。Claude Desktop と Claude Code の間で設定を共有し、効率的な開発環境を構築できます。

## 設定ファイル場所 | Configuration File Locations

### Claude Desktop

**macOS:**
```
~/Library/Application Support/Claude/claude_desktop_config.json
```

**Windows:**
```
%userprofile%\AppData\Roaming\Claude\claude_desktop_config.json
```

**Linux:** 
現在未対応

### Claude Code

**プロジェクトレベル:**
```
プロジェクトルート/.mcp.json
プロジェクトルート/.claude/settings.local.json
```

**ユーザーレベル:**
Claude Code の設定は `claude config` コマンドで管理

## シームレス移行 | Seamless Migration

### Claude Desktop → Claude Code

Claude Code には Claude Desktop の MCP 設定を直接インポートする機能があります：

```bash
# 基本インポート
claude mcp add-from-claude-desktop

# グローバル設定としてインポート
claude mcp add-from-claude-desktop -s global
```

**対応環境:**
- macOS
- Windows Subsystem for Linux (WSL)

**インポート時の動作:**
- サーバー名は Claude Desktop と同じ名前を保持
- 名前重複時は数値サフィックス追加（例：`server_1`）
- 対話式ダイアログでインポート対象を選択可能

## 設定ファイル構造 | Configuration File Structure

### 基本構造
```json
{
  "mcpServers": {
    "server-name": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-name"],
      "env": {
        "API_KEY": "your-api-key"
      }
    }
  }
}
```

### プロジェクトスコープ設定例 (.mcp.json)
```json
{
  "mcpServers": {
    "filesystem": {
      "type": "stdio", 
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "./src", "./docs"]
    },
    "git-tools": {
      "type": "stdio",
      "command": "npx", 
      "args": ["-y", "@modelcontextprotocol/server-git"]
    }
  }
}
```

## 設定スコープ階層 | Configuration Scope Hierarchy

1. **ローカルスコープ** - プロジェクト固有の個人設定
2. **プロジェクトスコープ** - チーム共有設定（バージョン管理対象）
3. **ユーザースコープ** - 全プロジェクト横断設定

優先順位：ローカル > プロジェクト > ユーザー

## 推奨MCPサーバー | Recommended MCP Servers

### 開発必須ツール
```bash
# ファイルシステム操作
claude mcp add filesystem -- npx -y @modelcontextprotocol/server-filesystem ~/Documents ~/Projects

# Git統合
claude mcp add git-tools -- npx -y @modelcontextprotocol/server-git

# Web検索・取得
claude mcp add web-search -- npx -y @modelcontextprotocol/server-web-search

# シーケンシャル思考支援
claude mcp add sequential-thinking -- npx -y mcp-sequentialthinking-tools
```

### AI Rules Repository 特化設定
```bash
# ドキュメント管理
claude mcp add docs-manager -s project -- npx -y @modelcontextprotocol/server-filesystem ./.claude/docs

# 規約チェック
claude mcp add lint-rules -s project -- npx -y @your-org/mcp-rules-checker

# テンプレート管理
claude mcp add templates -s project -- npx -y @modelcontextprotocol/server-filesystem ./.claude/templates
```

## 設定管理コマンド | Configuration Management Commands

### 基本操作
```bash
# MCPサーバー追加
claude mcp add <name> <command> [args...]

# 環境変数付きで追加
claude mcp add my-server -e API_KEY=123 -- /path/to/server arg1 arg2

# プロジェクトスコープで追加
claude mcp add shared-server -s project /path/to/server

# SSE transport使用
claude mcp add --transport sse sse-server https://example.com/sse-endpoint

# MCP認証管理
claude mcp auth <server-name>

# 設定確認
claude mcp list
```

### 設定ファイル直接編集
CLI ウィザードの代わりに設定ファイルを直接編集することで、より柔軟な制御が可能：

```bash
# プロジェクト設定ファイル作成
cat > .mcp.json << 'EOF'
{
  "mcpServers": {
    "project-tools": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@your-project/mcp-tools"]
    }
  }
}
EOF
```

## セキュリティ考慮事項 | Security Considerations

### 重要な注意点
- **サードパーティリスク**: サードパーティ MCP サーバーは慎重に選択
- **プロンプトインジェクション**: インターネット接続するサーバーは特に注意
- **機密情報保護**: API キーや認証情報の適切な管理
- **アクセス制限**: 必要最小限のディレクトリ・リソースのみアクセス許可

### 認証情報管理
```json
{
  "mcpServers": {
    "secure-api": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@vendor/secure-server"],
      "env": {
        "API_KEY": "${API_KEY}",
        "SECRET_TOKEN": "${SECRET_TOKEN}"
      }
    }
  }
}
```

## トラブルシューティング | Troubleshooting

### 一般的な問題
1. **設定反映されない**: アプリケーション再起動が必要
2. **パスエラー**: 絶対パスを使用、相対パスは避ける
3. **JSON構文エラー**: JSON バリデーターで確認
4. **権限エラー**: ファイル・ディレクトリのアクセス権限確認

### デバッグコマンド
```bash
# MCP接続状態確認
claude mcp status

# ログ出力
claude mcp logs <server-name>

# 設定ファイル検証
claude mcp validate
```

## 高度な設定例 | Advanced Configuration Examples

### マルチ環境対応
```json
{
  "mcpServers": {
    "dev-database": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-database"],
      "env": {
        "DATABASE_URL": "${DEV_DATABASE_URL}"
      }
    },
    "prod-readonly": {
      "type": "sse",
      "url": "https://api.example.com/mcp-readonly",
      "apiKey": "${PROD_API_KEY}"
    }
  }
}
```

### AI Rules Repository 統合例
```json
{
  "mcpServers": {
    "rules-validator": {
      "type": "stdio",
      "command": "node",
      "args": ["./scripts/mcp-rules-validator.js"],
      "workingDirectory": "${PROJECT_ROOT}"
    },
    "template-generator": {
      "type": "stdio", 
      "command": "npx",
      "args": ["-y", "@ai-rules/template-generator"],
      "env": {
        "RULES_PATH": "${PROJECT_ROOT}/.claude"
      }
    }
  }
}
```

## 関連ドキュメント | Related Documentation

- [Claude Code MCP 公式ドキュメント](https://docs.anthropic.com/en/docs/claude-code/mcp)
- [MCP プロトコル仕様](https://modelcontextprotocol.io/)
- [セキュリティガイドライン](.claude/docs/global/security.md)
- [プロジェクト構造規約](.claude/docs/structure/project_structure_conventions.md)

## 更新履歴 | Update History

- 2025-06-28: 初版作成、基本設定とシームレス移行手順を追加