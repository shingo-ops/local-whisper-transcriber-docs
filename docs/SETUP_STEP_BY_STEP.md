# 完全ローカル会議文字起こしツール - 実践セットアップガイド

このガイドは、実際のセットアップ手順を最初から最後まで詳しく説明します。

## 前提条件

- **Mac**（macOS）
- **ターミナル**が使える
- **インターネット接続**（初回セットアップのみ）
- **ディスク空き容量**: 10GB以上

---

## セットアップ手順（初回のみ）

### ステップ1: プロジェクトディレクトリに移動

ターミナルを開いて、以下を実行：

```bash
cd ~/sales-ops-with-claude/02_apps/whisper-tts-tool/local-whisper-ollama
```

### ステップ2: セットアップスクリプトを実行

```bash
./setup.sh
```

---

## よくあるパターンと対処法

### パターンA: Ollamaがインストールされていない

**表示されるメッセージ:**
```
【ステップ 1/6】Ollamaのインストール確認
! Ollama がインストールされていません

macOSの場合、以下のコマンドでインストールできます:
  brew install ollama

今すぐインストールしますか? (y/n):
```

**対処法:**
1. `y` を入力してEnterを押す
2. 自動的に `brew install ollama` が実行される
3. 完了まで待つ（1-2分）

**もしエラーが出た場合:**

手動でインストール：
```bash
brew install ollama
```

その後、セットアップスクリプトを再実行：
```bash
./setup.sh
```

---

### パターンB: Homebrewがインストールされていない

**エラーメッセージ:**
```
zsh: command not found: brew
```

**対処法:**

Homebrewをインストール：
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

完了したら、もう一度：
```bash
./setup.sh
```

---

### パターンC: セットアップスクリプトが途中で終了した

**状況:**
- Ollamaのインストールで `n` を選んだ
- エラーが出て途中で止まった

**対処法:**

#### 1. Ollamaを手動でインストール

```bash
brew install ollama
```

#### 2. Ollamaサーバーを起動

```bash
ollama serve &
```

**表示例:**
```
time=2026-03-11T15:12:06.823+09:00 level=INFO source=routes.go:1713 msg="Listening on 127.0.0.1:11434 (version 0.17.7)"
```

これが表示されればOK！

#### 3. 必要なモデルをダウンロード

**LLMモデル（約4.7GB、10-15分）:**
```bash
ollama pull qwen2.5:7b
```

**進行状況が表示されます:**
```
pulling manifest
pulling 2bada8a74506:   3% ▕███     ▏  140 MB/4.7 GB  5.2 MB/s  14m44s
pulling 2bada8a74506:  10% ▕█████   ▏  470 MB/4.7 GB  5.2 MB/s  13m12s
...
pulling 2bada8a74506: 100% ▕████████▏ 4.7 GB/4.7 GB
verifying sha256 digest
writing manifest
success
```

**埋め込みモデル（約700MB、1-2分）:**
```bash
ollama pull mxbai-embed-large
```

#### 4. セットアップスクリプトを再実行

```bash
./setup.sh
```

今度は、Ollamaとモデルが既にあるので、残りのステップ（Python環境構築）だけが実行されます。

---

## セットアップ完了の確認

以下のメッセージが表示されればセットアップ完了です：

```
==========================================
セットアップ完了！
==========================================

使い方:
  1. 仮想環境を有効化:
     source venv/bin/activate

  2. インタラクティブモードで起動:
     python main.py --interactive

  3. CLIモードで音声ファイルを処理:
     python main.py path/to/audio.mp3

詳しい使い方は README.md を参照してください
```

---

## セットアップ後の確認

### 1. Ollamaが起動しているか確認

```bash
ollama list
```

**正常な場合の表示:**
```
NAME                    ID              SIZE      MODIFIED
qwen2.5:7b             2bada8a74506    4.7 GB    2 minutes ago
mxbai-embed-large      468836162de7    669 MB    1 minute ago
```

もし `command not found` が出たら：
```bash
ollama serve &
```

### 2. 仮想環境が作成されているか確認

```bash
ls -la venv/
```

ディレクトリが表示されればOK。

### 3. 依存パッケージが入っているか確認

```bash
source venv/bin/activate
pip list | grep -E "faster-whisper|ollama|chromadb"
```

以下のようなパッケージが表示されればOK：
```
faster-whisper    1.0.0
ollama            0.1.0
chromadb          0.4.0
```

---

## トラブルシューティング

### エラー: `source: no such file or directory: venv/bin/activate`

**原因:** 仮想環境がまだ作成されていない

**解決策:**
```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### エラー: `zsh: command not found: python`

**原因:** macOSでは `python3` を使う必要がある

**解決策:**
```bash
# python3で仮想環境を作成
python3 -m venv venv

# 仮想環境を有効化
source venv/bin/activate

# この中では python が使える
python --version
```

### エラー: モデルのダウンロードが途中で止まる

**原因:** ネットワークの問題

**解決策:**
```bash
# Ctrl+C で中断

# もう一度実行（途中から再開される）
ollama pull qwen2.5:7b
```

### エラー: `Error: listen tcp 127.0.0.1:11434: bind: address already in use`

**原因:** Ollamaサーバーが既に起動している

**解決策:**
```bash
# 問題ありません。既に起動しているのでそのまま続行
ollama list  # 確認
```

---

## 所要時間の目安

| ステップ | 所要時間 |
|---------|---------|
| Homebrewインストール | 5-10分（初回のみ） |
| Ollamaインストール | 1-2分 |
| qwen2.5:7bダウンロード | 10-15分 |
| mxbai-embed-largeダウンロード | 1-2分 |
| Python環境構築 | 2-3分 |
| **合計** | **15-30分** |

---

## ディスク使用量

セットアップ完了後の使用容量：

| 項目 | サイズ |
|------|--------|
| qwen2.5:7b | 4.7 GB |
| mxbai-embed-large | 669 MB |
| faster-whisperモデル | 約3 GB（初回使用時） |
| Python仮想環境 | 500 MB |
| **合計** | **約9 GB** |

---

## 次のステップ

セットアップが完了したら、[使い方ガイド（HOW_TO_USE.md）](./HOW_TO_USE.md)に進んでください。

または、すぐに試したい場合：

```bash
./run.sh
```

これだけで起動します！
