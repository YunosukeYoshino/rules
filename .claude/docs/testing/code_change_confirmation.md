## コード変更後の確認

1. ビルドの確認
```bash
bun run build
```

2. 変更したファイルのユニットテスト実行
```bash
# 特定のテストファイルを実行
bun test tests/unit/components/Button.test.tsx

# 特定のディレクトリ内の全テストを実行
bun test tests/unit/components/
```

3. リントとフォーマットの確認
```bash
bun run biome:check
bun run biome:format
