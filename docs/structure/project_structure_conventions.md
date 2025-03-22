## 開発環境設定

### .vscode ディレクトリ

プロジェクトには以下の .vscode 設定ファイルを含めます。これにより、チーム全体で一貫した開発環境を確保します。

#### settings.json

```json
{
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.organizeImports.biome": true,
    "source.fixAll.biome": true
  },
  "editor.defaultFormatter": "biomejs.biome",
  "typescript.tsdk": "node_modules/typescript/lib",
  "typescript.enablePromptUseWorkspaceTsdk": true,
  "css.lint.unknownAtRules": "ignore",
  "tailwindCSS.experimental.classRegex": [
    ["cva\\(([^)]*)\\)", "[\"'`]([^\"'`]*).*?[\"'`]"],
    ["cn\\(([^)]*)\\)", "[\"'`]([^\"'`]*).*?[\"'`]"]
  ]
}
```

#### extensions.json

```json
{
  "recommendations": [
    "biomejs.biome",
    "bradlc.vscode-tailwindcss",
    "ms-vscode.vscode-typescript-next",
    "aaron-bond.better-comments",
    "dbaeumer.vscode-eslint",
    "mskelton.workspace-formatter",
    "pustelto.bracketeer",
    "naumovs.color-highlight",
    "paulmolluzzo.convert-css-in-js",
    "streetsidesoftware.code-spell-checker"
  ]
}
```

#### launch.json

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Next.js: debug server-side",
      "type": "node-terminal",
      "request": "launch",
      "command": "bun run dev"
    },
    {
      "name": "Next.js: debug client-side",
      "type": "chrome",
      "request": "launch",
      "url": "http://localhost:3000"
    },
    {
      "name": "Next.js: debug full stack",
      "type": "node-terminal",
      "request": "launch",
      "command": "bun run dev",
      "serverReadyAction": {
        "pattern": "- Local:.+(https?://.+)",
        "uriFormat": "%s",
        "action": "debugWithChrome"
      }
    }
  ]
}
```


プロジェクトルートに `biome.json` ファイルを配置し、チーム全体で一貫したコーディングスタイルとルールを確保します。

```json
{
  "$schema": "https://biomejs.dev/schemas/1.5.3/schema.json",
  "organizeImports": {
    "enabled": true
  },
  "linter": {
    "enabled": true,
    "rules": {
      "recommended": true,
      "correctness": {
        "noUnusedVariables": "error",
        "useExhaustiveDependencies": "error"
      },
      "suspicious": {
        "noExplicitAny": "error",
        "noConsoleLog": "warn"
      },
      "style": {
        "useConst": "error",
        "useImportType": "error",
        "useTemplate": "error"
      },
      "a11y": {
        "useButtonType": "error",
        "useAltText": "error"
      }
    },
    "ignore": [
      "node_modules",
      ".next",
      "dist",
      "public"
    ]
  },
  "formatter": {
    "enabled": true,
    "indentStyle": "space",
    "indentWidth": 2,
    "lineWidth": 100,
    "ignore": [
      "node_modules",
      ".next",
      "dist",
      "public",
      "*.md"
    ]
  },
  "javascript": {
    "parser": {
      "unsafeParameterDecoratorsEnabled": true
    },
    "formatter": {
      "quoteStyle": "single",
      "trailingComma": "es5",
      "semicolons": "always"
    }
  }
}
