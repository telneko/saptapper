---
name: analyze-code
description: saptapper のソースファイルやコンポーネントを詳細解析する。コードの仕組み、実装の詳細、アーキテクチャを理解したい時に使用。
allowed-tools: Read, Grep, Glob, Task
argument-hint: [component-name]
---

# コード解析スキル

saptapper のソースコードを詳細に解析します。

## 使用方法

引数でファイル名やコンポーネント名を指定してください。

例:
- `/analyze-code mp2k_driver` - ドライバ検出ロジックを解析
- `/analyze-code byte_pattern` - パターンマッチングを解析
- `/analyze-code gsf_writer` - GSF出力処理を解析

## 主要コンポーネント

| コンポーネント | 説明 |
|---------------|------|
| mp2k_driver | MusicPlayer2000 ドライバ検出（コア機能） |
| byte_pattern | バイトパターンマッチング |
| saptapper | メイン処理ロジック |
| gsf_writer | GSF ファイル出力 |
| psf_writer | PSF ファイル出力 |
| cartridge | ROM 読み込み |
| arm | ARM 命令ユーティリティ |

## 解析内容

1. ファイル構造の概要
2. 主要な関数・クラスの役割
3. 他コンポーネントとの依存関係
4. 重要なアルゴリズムの説明

## 対象

$ARGUMENTS
