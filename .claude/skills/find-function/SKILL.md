---
name: find-function
description: saptapper のソースコード内で関数やクラスを検索して詳細を表示する。特定の関数、クラス、型定義を探したい時に使用。
allowed-tools: Read, Grep, Glob, mcp__plugin_serena_serena__find_symbol
argument-hint: [function-or-class-name]
---

# 関数・クラス検索スキル

saptapper のソースコード内で関数やクラスを検索します。

## 使用方法

引数で検索したい関数名やクラス名を指定してください。

例:
- `/find-function FindM4aSoundMain` - 特定の関数を検索
- `/find-function BytePattern` - クラスを検索
- `/find-function agbptr` - 型定義を検索

## 検索対象ディレクトリ

- `src/saptapper/` - メインソースコード
- `src/` - エントリポイント等

## 出力内容

1. 関数/クラスの定義場所
2. シグネチャ
3. 実装内容（必要に応じて）
4. 参照箇所

## 検索対象

$ARGUMENTS
