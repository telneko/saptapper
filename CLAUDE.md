# Saptapper - AI Assistant Guide

このドキュメントはAIアシスタントがプロジェクトを理解するためのガイドです。

## セッション開始時の必須事項

1. **Serena メモリを確認**: `saptapper-project-analysis.md` にプロジェクト解析結果が保存されています
2. **Agent Skills を活用**: `.claude/skills/` に定義されたスキルを使用してください

## 利用可能な Agent Skills

| スキル | 説明 | 呼び出し |
|--------|------|----------|
| build | プロジェクトをビルド | `/build` |
| analyze-code | コンポーネントを詳細解析 | `/analyze-code [component]` |
| find-function | 関数・クラスを検索 | `/find-function [name]` |
| explain-arm | ARM命令を解説 | `/explain-arm [topic]` |
| project-status | プロジェクト状態確認 | `/project-status` |

## プロジェクト概要

**Saptapper** は GBA ROM から音楽を抽出する GSF リッパーツールです。

- **言語**: C++17
- **ビルド**: CMake
- **ライセンス**: LGPL v3

## クイックリファレンス

### ビルドコマンド

```bash
# Linux
mkdir build && cd build
cmake .. && cmake --build .

# Windows (要 Visual Studio)
mkdir build && cd build
cmake .. && msbuild saptapper.sln
```

### ディレクトリ構造

```
src/
├── main.cpp              # エントリポイント
├── saptapper/            # コアロジック
│   ├── mp2k_driver.*     # MusicPlayer2000 ドライバ検出（最重要）
│   ├── byte_pattern.*    # パターンマッチング
│   ├── saptapper.*       # メイン処理
│   ├── gsf_writer.*      # GSF 出力
│   ├── psf_writer.*      # PSF 出力
│   ├── cartridge.*       # ROM 読み込み
│   └── arm.hpp           # ARM 命令ユーティリティ
├── 3rdparty/include/     # サードパーティヘッダ
└── asm/m4a-gsf.s         # GSF ドライバ (ARM アセンブリ)
```

### 主要コンポーネント

| コンポーネント | ファイル | 説明 |
|---------------|----------|------|
| ドライバ検出 | `mp2k_driver.*` | ROM 内の MusicPlayer2000 関数を特定 |
| パターンマッチ | `byte_pattern.*` | バイト列のシグネチャスキャン |
| GSF 生成 | `gsf_writer.*`, `saptapper.*` | miniGSF ファイル出力 |
| ROM 読込 | `cartridge.*` | GBA カートリッジイメージ処理 |

## コーディング規約

- **フォーマット**: `src/.clang-format` に従う
- **命名規則**: snake_case (変数・関数), PascalCase (クラス)
- **ヘッダガード**: `#pragma once` を使用

## 依存関係

- **zlib**: 圧縮（Windows用はプリコンパイル済み同梱）
- **ヘッダオンリー**: args.hxx, strict_fstream.hpp, zstr.hpp

## 注意事項

### テスト
- ユニットテストは存在しない
- 変更時は手動でビルド確認が必要

### ARM アセンブリ
- `src/asm/m4a-gsf.s` は GSF ドライバの ARM コード
- 変更には ARM アーキテクチャの知識が必要

### Windows ビルド
- zlib は `dependencies/zlib/lib/` から自動リンク
- x86/x64 両アーキテクチャ対応

## 開発フロー

1. 変更前に `git status` で状態確認
2. コード変更
3. `cmake --build build` でビルド確認
4. 実際の GBA ROM でテスト（可能であれば）

## 関連リソース

- [GSF フォーマット仕様](http://www.vgmpf.com/Wiki/index.php/GSF)
- [MusicPlayer2000 / Sappy](https://www.romhacking.net/utilities/881/)
