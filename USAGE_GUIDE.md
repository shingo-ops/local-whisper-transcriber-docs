# 使い方ガイド

## 初回セットアップ vs 毎回の起動

### 🔧 初回セットアップ（1回だけ）

```bash
cd ~/sales-ops-with-claude/02_apps/whisper-tts-tool/local-whisper-ollama
./setup.sh
```

**所要時間:** 5-15分（モデルのダウンロードに時間がかかります）

**何が起こる？**
- Ollamaのインストール確認
- qwen2.5:7b モデルのダウンロード（約4GB）
- mxbai-embed-large モデルのダウンロード（約700MB）
- Python仮想環境の作成
- 必要なPythonパッケージのインストール

**これは1度だけ実行すればOKです！**

---

### 🚀 起動方法（毎回必要）

ターミナルを新しく開くたびに、以下のいずれかの方法で起動します。

#### **方法1: 簡単起動スクリプト（推奨）**

```bash
cd ~/sales-ops-with-claude/02_apps/whisper-tts-tool/local-whisper-ollama
./run.sh
```

**これが一番簡単です！**

#### **方法2: さらに簡単に（エイリアス設定）**

毎回 `cd` するのも面倒な場合、エイリアスを設定:

```bash
# ~/.zshrc に以下を追加
echo 'alias meeting="cd ~/sales-ops-with-claude/02_apps/whisper-tts-tool/local-whisper-ollama && ./run.sh"' >> ~/.zshrc

# 設定を反映
source ~/.zshrc
```

設定後は、ターミナルでこれだけ:

```bash
meeting
```

#### **方法3: 手動起動**

```bash
cd ~/sales-ops-with-claude/02_apps/whisper-tts-tool/local-whisper-ollama
source venv/bin/activate
python main.py --interactive
```

---

## 典型的な使い方

### シナリオ1: 今から会議を録音

```bash
# 起動
./run.sh

# メニューで「2」を選択
# 🎤 今すぐ録音して処理（リアルタイム）

# デフォルトのマイクを使用: y
# 録音方法: 1（自分で停止するまで録音）

# Enterを押して録音開始
# 会議中...
# 会議終了後、Ctrl+C で停止

# 自動的に文字起こし・議事録生成が始まる
# 完了！
```

### シナリオ2: 既存の音声ファイルを処理

```bash
# 起動
./run.sh

# メニューで「1」を選択
# 音声ファイルを処理

# ファイルパスを入力（ドラッグ&ドロップでOK）
音声ファイルのパスを入力: /Users/yourname/Downloads/meeting.mp3

# 言語: ja（Enterでデフォルト）
# Google Sheetsに保存: n（Enterでデフォルト）

# 処理開始...
# 完了！
```

### シナリオ3: OpenAI Whisperアプリの結果をローカルLLMで処理

```bash
# 1. OpenAI WhisperアプリのWebで音声を文字起こし
# 2. スプレッドシートに結果が保存される

# 3. ローカルツールを起動
./run.sh

# メニューで「6」を選択
# Google Sheetsから未処理データを取得して処理

# 自動的に未処理の文字起こし結果を取得
# ローカルLLMで議事録生成
# スプレッドシートに結果を追記
```

---

## よくある質問

### Q1: 毎回セットアップが必要？

**A:** いいえ、セットアップ（`./setup.sh`）は**初回のみ**です。

2回目以降は `./run.sh` だけで起動できます。

### Q2: 仮想環境の有効化は毎回必要？

**A:** はい、ターミナルを新しく開くたびに必要です。

ただし `./run.sh` を使えば、自動的に有効化されます。

### Q3: Ollamaは毎回起動が必要？

**A:** Ollamaは通常バックグラウンドで常駐しています。

確認方法:
```bash
ollama list  # モデル一覧が表示されればOK
```

起動していない場合:
```bash
ollama serve &
```

### Q4: モデルのダウンロードは毎回必要？

**A:** いいえ、初回のセットアップでダウンロードしたモデルは永続的に保存されます。

削除しない限り、再ダウンロードは不要です。

### Q5: ディスク容量はどれくらい必要？

- qwen2.5:7b: 約4GB
- mxbai-embed-large: 約700MB
- faster-whisperモデル: 約3GB（初回使用時に自動ダウンロード）
- 依存パッケージ: 約500MB

**合計: 約8-10GB**

---

## トラブルシューティング

### エラー: `source: no such file or directory: venv/bin/activate`

**原因:** まだセットアップが完了していない

**解決策:**
```bash
./setup.sh
```

### エラー: `zsh: command not found: python`

**原因:** macOSでは `python3` コマンドを使う必要がある

**解決策:**
```bash
# setup.shを使う（自動的にpython3を使用）
./setup.sh

# または手動で
python3 -m venv venv
source venv/bin/activate
```

### エラー: `ollama: command not found`

**原因:** Ollamaがインストールされていない

**解決策:**
```bash
brew install ollama
```

### エラー: モデルが見つからない

**原因:** モデルがダウンロードされていない

**解決策:**
```bash
ollama serve &
ollama pull qwen2.5:7b
ollama pull mxbai-embed-large
```

---

## パフォーマンス最適化

### 処理を高速化したい

`.env` ファイルを編集:

```bash
# より小さいモデルを使用
WHISPER_MODEL=medium      # large-v3 の代わりに
OLLAMA_MODEL=qwen2.5:3b   # qwen2.5:7b の代わりに
```

### GPU を使いたい（NVIDIA）

`.env` ファイルを編集:

```bash
WHISPER_DEVICE=cuda
WHISPER_COMPUTE_TYPE=float16
```

### メモリ不足の場合

```bash
# より軽量な設定
WHISPER_MODEL=small
WHISPER_COMPUTE_TYPE=int8
OLLAMA_MODEL=gemma2:2b
```

---

## 次のステップ

- RAGナレッジベースをカスタマイズ → `knowledge_base/` フォルダにMarkdownファイルを追加
- Google Sheets連携を設定 → `QUICKSTART.md` の「Google Sheets連携」セクション参照
- プロンプトをカスタマイズ → `src/ollama_processor.py` の `_get_default_user_prompt()` を編集
