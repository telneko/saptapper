---
name: project-status
description: saptapper プロジェクトの現在の状態を確認する。Git状態、ビルド状態、依存関係を確認したい時に使用。
disable-model-invocation: true
allowed-tools: Bash, Read, mcp__plugin_serena_serena__read_memory
---

# プロジェクト状態確認スキル

saptapper プロジェクトの現在の状態を確認します。

## 確認内容

1. **Git 状態**: 変更ファイル、ブランチ
2. **ビルド状態**: build ディレクトリの有無
3. **依存関係**: zlib の状態

## 実行手順

```bash
# Git 状態
git status
git branch --show-current

# ビルドディレクトリ確認
ls -la build/ 2>/dev/null || echo "build ディレクトリなし"

# 依存関係確認
ls dependencies/zlib/lib/
```

## メモリ参照

プロジェクト解析結果は Serena メモリ `saptapper-project-analysis.md` に保存されています。
