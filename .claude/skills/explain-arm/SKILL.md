---
name: explain-arm
description: ARM命令やアセンブリコードの解説。ARM アーキテクチャ、命令セット、m4a-gsf.s の内容を理解したい時に使用。
allowed-tools: Read, Grep, Glob, WebSearch
argument-hint: [instruction-or-topic]
---

# ARM 命令解説スキル

saptapper で使用されている ARM 関連のコードを解説します。

## 使用方法

引数で解説してほしい内容を指定してください。

例:
- `/explain-arm branch` - ブランチ命令の解説
- `/explain-arm m4a-gsf.s` - GSF ドライバアセンブリの解説
- `/explain-arm instruction 0xEA000000` - 特定の命令をデコード

## 対象ファイル

- `src/saptapper/arm.hpp` - ARM 命令ユーティリティ
- `src/asm/m4a-gsf.s` - GSF ドライバ (ARM アセンブリ)

## 解説内容

1. ARM 命令のエンコーディング
2. 条件コード
3. アドレッシングモード
4. saptapper での使用目的

## 解説対象

$ARGUMENTS
