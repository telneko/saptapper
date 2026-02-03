---
name: debug-detection
description: saptapper で ROM の検出が失敗した際の調査・修正フロー。--inspect で FAILED になる ROM の原因を特定し、パターン修正を行う。
allowed-tools: Bash, Read, Grep, Glob, Edit, Write
argument-hint: [ROM-file-path]
---

# 検出失敗デバッグスキル

saptapper で ROM の検出が失敗した際の調査・修正を行います。

## 使用方法

```
/debug-detection path/to/rom.gba
```

## 調査フロー

### Step 1: 検出状態を確認

```bash
saptapper.exe --inspect <ROM>
```

出力例（失敗時）:
```
Status: FAILED

|Name            |Address / Value |
|----------------|----------------|
|m4aSoundVSync   |null            |  ← 検出失敗
|m4aSoundInit    |null            |  ← 検出失敗
|m4aSoundMain    |0x81c10a4       |
|m4aSongNumStart |0x81c10b0       |
|song_table      |0x8467f0c       |
|len(song_table) |347             |
```

### Step 2: 失敗箇所を特定

検出の依存関係:
```
FindSelectSongFn (m4aSongNumStart)
    ↓
FindSongTable
    ↓
FindMainFn (m4aSoundMain)
    ↓
FindInitFn (m4aSoundInit)      ← ここが失敗すると
    ↓
FindVSyncFn (m4aSoundVSync)    ← ここも連鎖的に失敗
```

### Step 3: ROM のバイトパターンを確認

検出済みアドレスの前方をダンプして実際のパターンを確認:

```bash
# MainFn (0x81c10a4 → オフセット 0x1c10a4) の前方 0x100 バイトを確認
xxd -s 0x1c0fa4 -l 0x120 rom.gba
```

### Step 4: 既存パターンと比較

`src/saptapper/mp2k_driver.cpp` の該当関数を確認:

| 関数 | 検索パターン |
|------|-------------|
| FindSelectSongFn | 30バイト固定パターン |
| FindInitFn | `70 b5 ?? 48` または `f0 b5 47 46` |
| FindVSyncFn | 複数パターン（通常版/Puyo Pop版） |

### Step 5: パターン修正

差異が即値オフセット（コンパイル依存）の場合、`BytePattern` でマスク:

```cpp
// 固定パターン（マッチしない場合がある）
"\x70\xb5\x14\x48"sv

// マスク付きパターン（3バイト目をワイルドカード）
BytePattern{"\x70\xb5\x00\x48\x02\x21\x49\x42\x08\x40"sv, "xx?xxxxxxx"sv}
```

### Step 6: ビルド＆テスト

```bash
# ビルド
cmake --build build --config Release

# テスト
saptapper.exe --inspect <ROM>
# Status: OK を確認

# 抽出テスト
mkdir -p output/<romname>
saptapper.exe <ROM> -d output/<romname>
```

## 主な失敗パターンと対処法

| 症状 | 原因 | 対処 |
|------|------|------|
| InitFn が null | ldr 命令の即値オフセット違い | マスク付きパターンに変更 |
| SelectSongFn が null | ドライババージョン違い | 新パターン追加が必要 |
| VSyncFn が null | InitFn 失敗の連鎖 or 別パターン | InitFn 修正 or VSync パターン追加 |
| song_count が 0 | テーブル終端判定の問題 | ReadSongCount の修正 |

## 対象 ROM

$ARGUMENTS
