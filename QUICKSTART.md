# クイックスタートガイド

このガイドに従えば、5分で完全ローカル会議文字起こしツールを使い始められます。

## 前提条件

- Python 3.9以上
- 8GB以上のメモリ
- 10GB以上のディスク空き容量

## 1. セットアップ（初回のみ）

```bash
cd ~/sales-ops-with-claude/02_apps/whisper-tts-tool/local-whisper-ollama

# セットアップスクリプトを実行
./setup.sh
```

このスクリプトが自動的に以下を実行します:
- Ollamaのインストール確認
- 必要なモデルのダウンロード（qwen2.5:7b、mxbai-embed-large）
- Python仮想環境の作成
- 依存パッケージのインストール
- .envファイルの作成

**これは初回のみ実行すればOKです！**

## 2. 起動（2回目以降はこれだけ）

### 方法A: 簡単起動スクリプト（推奨）

```bash
cd ~/sales-ops-with-claude/02_apps/whisper-tts-tool/local-whisper-ollama
./run.sh
```

これだけで起動します！

### 方法B: 手動起動

```bash
cd ~/sales-ops-with-claude/02_apps/whisper-tts-tool/local-whisper-ollama
source venv/bin/activate
python main.py --interactive
```

## 3. 使ってみる

### 音声ファイルを処理

1. メニューで「1」を選択
2. 音声ファイルのパスを入力（例: `~/Downloads/meeting.mp3`）
3. 言語を選択（日本語の場合は `ja` または Enter）
4. 処理完了まで待つ

結果は `output/` ディレクトリに保存されます:
- `{ファイル名}_transcript.txt` - 文字起こし結果
- `{ファイル名}_minutes.md` - 議事録

### 処理時間の目安

| 音声長 | 文字起こし | 議事録生成 | 合計 |
|--------|----------|----------|------|
| 10分 | 約3分 | 約10秒 | 約3分10秒 |
| 30分 | 約10分 | 約30秒 | 約10分30秒 |
| 1時間 | 約20分 | 約1分 | 約21分 |

## 4. RAG（ナレッジベース）を使う

専門用語や組織情報を追加して精度向上:

```bash
# knowledge_base/ディレクトリにMarkdownファイルを作成
echo "## MRR
Monthly Recurring Revenue（月次経常収益）" > knowledge_base/my_terms.md

# メニューで「4」を選択してRAGを再インデックス
```

次回から、議事録生成時に自動的に参照されます。

## 5. 既存アプリとの統合（オプション）

既存のWhisper TTSツールと連携する場合:

### Google Sheets認証設定

1. [Google Cloud Console](https://console.cloud.google.com/)でOAuth認証情報を取得
2. `credentials.json`をダウンロード
3. `.env`ファイルを編集:

```bash
GOOGLE_CREDENTIALS_PATH=./credentials.json
SPREADSHEET_ID=your_spreadsheet_id_here
SHEET_NAME=音声解析ログ
```

### 使い方

```bash
python main.py --interactive
# メニューで「5」を選択
# Sheetsの未処理データから自動的に議事録生成
```

## トラブルシューティング

### エラー: `ollama: command not found`

```bash
# macOS
brew install ollama

# Linux
curl -fsSL https://ollama.com/install.sh | sh
```

### エラー: モデルが見つからない

```bash
ollama serve &
ollama pull qwen2.5:7b
ollama pull mxbai-embed-large
```

### メモリ不足の場合

より軽量なモデルを使用:

```bash
# .envファイルを編集
OLLAMA_MODEL=qwen2.5:3b
WHISPER_MODEL=medium
```

## 次のステップ

詳しい使い方は [README.md](README.md) を参照してください。

特に以下のセクションが役立ちます:
- パフォーマンスチューニング
- ハイブリッド運用（OpenAI + ローカル）
- RAG機能の活用方法
