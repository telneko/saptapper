# Saptapper プロジェクト解析結果

## 概要

**Saptapper** は、GBA (Game Boy Advance) ROM ファイルから音楽を自動抽出する GSF (Game Sound Format) リッパーツール。MusicPlayer2000 ドライバ（m4a / Sappy）を使用するゲームから音楽データを抽出する。

- **オリジナル作者**: Caitsith2
- **v2.0 再実装**: loveemu
- **ライセンス**: LGPL v3

## ディレクトリ構成

```
saptapper/
├── src/                   # ソースコード
│   ├── saptapper/         # メインのソースディレクトリ
│   ├── 3rdparty/include/  # サードパーティヘッダ (args.hxx, strict_fstream.hpp, zstr.hpp)
│   ├── asm/               # ARM アセンブリ (m4a-gsf.s)
│   └── main.cpp           # エントリポイント
├── dependencies/zlib/     # zlib ライブラリ (Windows用プリコンパイル済み)
├── CMakeLists.txt         # CMake ビルド設定
└── README.md              # ドキュメント
```

## 技術スタック

| 項目 | 内容 |
|------|------|
| **言語** | C++17 |
| **ビルドシステム** | CMake (minimum 2.8) |
| **依存ライブラリ** | zlib (圧縮) |
| **CI/CD** | Travis CI (Linux, g++-9), AppVeyor (Windows, VS2017) |
| **対応OS** | Linux (x86/x64), Windows (x86/x64) |

## 主要ソースファイル

| ファイル | 行数 | 役割 |
|----------|------|------|
| `mp2k_driver.cpp/.hpp` | ~700/300 | MusicPlayer2000 ドライバの検出・解析（コア機能） |
| `byte_pattern.cpp/.hpp` | ~800/100 | マスク付きバイトパターンマッチング |
| `saptapper.cpp/.hpp` | ~500/150 | メイン処理: 検査、GSF生成、重複削除 |
| `gsf_writer.cpp/.hpp` | ~200/150 | GSF ファイル出力（タグ・圧縮対応） |
| `psf_writer.cpp/.hpp` | ~300/150 | PSF フォーマット出力 |
| `cartridge.cpp/.hpp` | ~60/50 | GBA ROM ファイル読み込み・検証 |
| `arm.hpp` | ~50 | ARM 命令のパース・生成（ブランチ命令） |
| `types.hpp` | ~50 | 型定義 (agbptr_t, agbsize_t) |

## アーキテクチャの特徴

1. **パターンベース検出**: マスク付きバイトパターンで ROM 内の ARM 関数を探索
2. **ARM ISA 知識**: ブランチ命令の解析・生成が組み込まれている
3. **複数フォーマット対応**: GSF (Game Sound Format) と PSF (PlayStation Sound Format)
4. **モジュラー設計**: カートリッジ読込、ドライバ検出、出力書き込みが分離
5. **クロスプラットフォーム**: Windows 用にはプリコンパイル済み zlib を同梱

## ビルド方法

### Linux
```bash
mkdir build && cd build
cmake ..
cmake --build .
```

### Windows (Visual Studio)
```bash
mkdir build && cd build
cmake ..
msbuild saptapper.sln
```

## テスト状況

ユニットテストは存在しない。CI/CD でのビルド成功確認のみ。

## 依存関係

### 直接依存
- **zlib**: 圧縮ライブラリ
  - Linux: システム提供
  - Windows: `dependencies/zlib/lib/` にプリコンパイル済み

### ヘッダオンリー依存 (src/3rdparty/include/)
- **args.hxx**: コマンドライン引数パーサ
- **strict_fstream.hpp**: 厳密なエラーチェック付きファイルストリーム
- **zstr.hpp**: zlib ベースの圧縮ストリームラッパー
