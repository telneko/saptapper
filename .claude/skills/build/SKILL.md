---
name: build
description: saptapper プロジェクトをビルドする。ビルド、コンパイル、cmake を実行したい時に使用。
disable-model-invocation: true
allowed-tools: Bash, Read
argument-hint: [target]
---

# ビルドコマンド

saptapper プロジェクトをビルドします。

## 手順

1. build ディレクトリが存在するか確認
2. 存在しない場合は作成して cmake を実行
3. cmake --build でビルド

## 実行コマンド

```bash
# build ディレクトリ確認・作成
if [ ! -d build ]; then
    mkdir build
    cd build
    cmake ..
    cd ..
fi

# ビルド実行
cmake --build build
```

## 引数

- `$ARGUMENTS` - オプションでビルドターゲットを指定可能

## 注意

- Windows の場合は Visual Studio が必要
- zlib は Windows 用にプリコンパイル済みが同梱されている
