# 🎤 完全ローカル会議文字起こしツール - 使用説明書

**faster-whisper + Ollama で完全無料の議事録自動生成**

![Version](https://img.shields.io/badge/version-1.0.0-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Platform](https://img.shields.io/badge/platform-macOS-lightgrey)

> このリポジトリは**使用説明書専用**です。
> ツール本体は [sales-ops-with-claude](https://github.com/shingo-ops/sales-ops-with-claude) にあります。

---

## 📚 ドキュメント一覧

### 🔰 はじめに

| ドキュメント | 内容 | 対象者 |
|------------|------|--------|
| **[START_HERE.md](./START_HERE.md)** | **まず最初に読むファイル**<br>全体の案内とナビゲーション | すべての人 |
| **[BEGINNER_GUIDE.md](./BEGINNER_GUIDE.md)** | **ターミナル初心者向け完全ガイド**<br>コマンドの基礎から詳しく説明 | ターミナルが初めての人 |

### 📖 セットアップと使い方

| ドキュメント | 内容 |
|------------|------|
| [QUICKSTART.md](./QUICKSTART.md) | 5分で始めるクイックガイド |
| [SETUP_STEP_BY_STEP.md](./SETUP_STEP_BY_STEP.md) | 実践的なセットアップ手順（エラー対処法含む） |
| [HOW_TO_USE.md](./HOW_TO_USE.md) | 詳しい使い方・FAQ・実例 |
| [USAGE_GUIDE.md](./USAGE_GUIDE.md) | より詳しい使い方とトラブルシューティング |

---

## 💡 主な機能

### ✅ 完全ローカル・無料
- クラウドサービス不要で完全ローカル動作
- APIコスト0円
- データは完全にローカルで処理
- オフライン環境でも動作可能

### 高性能・高精度
- **faster-whisper**: CTranslate2ベースで最大4倍高速化
- **Kotoba Whisper V1.0**: 日本語会議に最適化（large-v3より6.3倍高速）
- **Ollama + qwen2.5:7b**: 実用レベルの議事録生成

### RAG機能
- ナレッジベースから関連情報を自動検索
- 専門用語や組織情報を参照して精度向上
- ChromaDB + Ollama埋め込みモデル使用

### 既存アプリとの統合
- Google Sheets連携でOpenAI Whisperアプリと併用可能
- 既存の文字起こし結果からローカルLLMで議事録生成
- 段階的な移行をサポート

## アーキテクチャ

```
音声ファイル
  ↓
faster-whisper (Kotoba Whisper V1.0)
  ↓
文字起こしテキスト
  ↓
RAGエンジン (ChromaDB)
  ↓ (関連情報を検索)
Ollama (qwen2.5:7b)
  ↓
議事録生成
  ↓
出力 (テキストファイル / Google Sheets)
```

## セットアップ

### 前提条件

- Python 3.9以上
- macOS / Linux / Windows
- メモリ: 8GB以上推奨
- ディスク: 10GB以上の空き容量

### 1. Ollamaのインストール

**macOS:**
```bash
brew install ollama
```

**Linux:**
```bash
curl -fsSL https://ollama.com/install.sh | sh
```

**Windows:**
[Ollama公式サイト](https://ollama.com/)からインストーラーをダウンロード

### 2. モデルのダウンロード

```bash
# Ollamaサーバーを起動（バックグラウンド）
ollama serve &

# LLMモデル（qwen2.5:7b推奨）
ollama pull qwen2.5:7b

# 埋め込みモデル（RAG用）
ollama pull mxbai-embed-large
```

### 3. Pythonパッケージのインストール

```bash
cd 02_apps/whisper-tts-tool/local-whisper-ollama

# 仮想環境作成
python -m venv venv

# 仮想環境を有効化
# macOS/Linux:
source venv/bin/activate
# Windows:
venv\Scripts\activate

# 依存パッケージインストール
pip install -r requirements.txt
```

### 4. 環境変数の設定

```bash
# .envファイルを作成
cp .env.example .env

# .envファイルを編集
nano .env  # または任意のエディタ
```

**基本設定例（.env）:**
```bash
# Whisper設定
WHISPER_MODEL=large-v3
WHISPER_DEVICE=cpu
WHISPER_COMPUTE_TYPE=int8

# Ollama設定
OLLAMA_MODEL=qwen2.5:7b
OLLAMA_HOST=http://localhost:11434
OLLAMA_TEMPERATURE=0.3

# RAG設定
RAG_ENABLED=true
RAG_EMBEDDING_MODEL=mxbai-embed-large
KNOWLEDGE_BASE_DIR=./knowledge_base

# 出力設定
OUTPUT_DIR=./output
```

### 5. Google Sheets連携（オプション）

Google Sheets連携を使う場合:

1. [Google Cloud Console](https://console.cloud.google.com/)でプロジェクト作成
2. Google Sheets APIを有効化
3. OAuth 2.0認証情報（デスクトップアプリ）を作成
4. `credentials.json`をダウンロードしてプロジェクトルートに配置
5. `.env`に以下を追加:

```bash
GOOGLE_CREDENTIALS_PATH=./credentials.json
SPREADSHEET_ID=your_spreadsheet_id_here
SHEET_NAME=音声解析ログ
```

## 使い方

### クイックスタート

**初回のみ:**
```bash
cd ~/sales-ops-with-claude/02_apps/whisper-tts-tool/local-whisper-ollama
./setup.sh
```

**2回目以降（毎回これだけ）:**
```bash
cd ~/sales-ops-with-claude/02_apps/whisper-tts-tool/local-whisper-ollama
./run.sh
```

### インタラクティブモード（推奨）

```bash
python main.py --interactive
```

メニューが表示され、以下の操作が可能:
1. 音声ファイルを処理（文字起こし + 議事録生成）
2. 文字起こしのみ実行
3. 既存の文字起こしから議事録生成
4. RAGナレッジベースを再インデックス
5. Google Sheetsから未処理データを取得して処理

### CLIモード

```bash
# 音声ファイルを処理
python main.py path/to/audio.mp3

# 言語を指定
python main.py path/to/audio.mp3 -l ja

# Google Sheetsに保存
python main.py path/to/audio.mp3 --sheets

# RAG無効化
python main.py path/to/audio.mp3 --no-rag
```

### ナレッジベースの活用

`knowledge_base/`ディレクトリにMarkdownファイルを配置:

```bash
knowledge_base/
├── terms.md          # 専門用語集
├── members.md        # メンバー情報
└── project_info.md   # プロジェクト情報
```

RAGエンジンが自動的にインデックス化し、議事録生成時に参照します。

## 性能比較

### 処理速度（30分会議の場合）

| 処理 | OpenAI API | ローカル（本ツール） |
|------|-----------|------------------|
| 文字起こし | 約3分 | 約10分 |
| 議事録生成 | 約10秒 | 約30秒 |
| 合計 | 約3分10秒 | 約10分30秒 |

### コスト比較

| 処理内容 | OpenAI API | ローカル |
|---------|-----------|---------|
| 30分会議の文字起こし | 約27円 | **0円** |
| 議事録生成 | 約50円 | **0円** |
| 合計 | 約77円 | **0円** |

### 品質比較

| 項目 | OpenAI Whisper | faster-whisper |
|------|---------------|---------------|
| 文字起こし精度 | ★★★★★ | ★★★★☆ |
| 日本語特化 | ★★★★☆ | ★★★★★ |
| 専門用語対応 | ★★★☆☆ | ★★★★★ (RAG) |

## ハイブリッド運用

既存のOpenAI Whisperアプリと併用する場合:

### パターン1: 文字起こしはクラウド、議事録はローカル

```bash
# 1. Webアプリで音声を文字起こし（OpenAI Whisper API使用）
# 2. Sheetsから未処理データを取得して議事録生成

python main.py --interactive
# メニューで「5. Google Sheetsから未処理データを取得して処理」を選択
```

**メリット:**
- 文字起こし精度は最高品質
- 議事録生成コストは0円
- 約65%のコスト削減

### パターン2: 完全ローカル

```bash
# すべてローカルで処理
python main.py path/to/audio.mp3
```

**メリット:**
- 完全無料
- プライバシー完全保護
- オフライン動作

## プロジェクト構造

```
local-whisper-ollama/
├── main.py                    # メインCLIアプリケーション
├── requirements.txt           # 依存パッケージ
├── .env.example              # 環境変数サンプル
├── README.md                 # このファイル
├── src/
│   ├── __init__.py
│   ├── whisper_transcriber.py  # faster-whisper文字起こし
│   ├── ollama_processor.py     # Ollama議事録生成
│   ├── rag_engine.py           # RAG検索エンジン
│   └── sheets_integration.py   # Google Sheets連携
├── knowledge_base/            # RAGナレッジベース
│   ├── sample_terms.md
│   └── sample_members.md
├── output/                    # 出力ディレクトリ
└── tests/                     # テスト
```

## トラブルシューティング

### Ollamaに接続できない

```bash
# Ollamaが起動しているか確認
ollama list

# 起動していない場合
ollama serve
```

### モデルが見つからない

```bash
# インストール済みモデルを確認
ollama list

# モデルをダウンロード
ollama pull qwen2.5:7b
ollama pull mxbai-embed-large
```

### faster-whisperのインストールエラー

```bash
# システムライブラリが必要な場合（Ubuntu/Debian）
sudo apt-get update
sudo apt-get install ffmpeg

# macOS
brew install ffmpeg
```

### メモリ不足エラー

`.env`ファイルで軽量モデルに変更:

```bash
# より軽量なモデル
OLLAMA_MODEL=qwen2.5:3b  # または gemma2:2b
WHISPER_MODEL=medium     # または small
```

### Google Sheets認証エラー

1. `credentials.json`が正しく配置されているか確認
2. Google Cloud ConsoleでSheets APIが有効化されているか確認
3. `token.json`を削除して再認証:

```bash
rm token.json
python main.py --sheets path/to/audio.mp3
```

## パフォーマンスチューニング

### GPU使用（NVIDIA）

```bash
# .envファイルで設定
WHISPER_DEVICE=cuda
WHISPER_COMPUTE_TYPE=float16
```

### 処理速度を優先

```bash
# より小さいモデルを使用
WHISPER_MODEL=medium
OLLAMA_MODEL=qwen2.5:3b
```

### 精度を優先

```bash
# より大きいモデルを使用
WHISPER_MODEL=large-v3-ja
OLLAMA_MODEL=qwen2.5:14b
WHISPER_COMPUTE_TYPE=float32
```

## 参考資料

- [Zenn記事: ローカルLLMで会議文字起こし](https://zenn.dev/okamyuji/articles/local-meeting-transcriber-with-whisper)
- [faster-whisper GitHub](https://github.com/SYSTRAN/faster-whisper)
- [Ollama公式サイト](https://ollama.com/)
- [Kotoba Whisper](https://huggingface.co/kotoba-tech/kotoba-whisper-v1.0)

## ライセンス

MIT License

## 作成者

谷澤真吾

## 更新履歴

### v1.0.0 (2026-03-11)
- 初回リリース
- faster-whisper + Ollama統合
- RAG機能実装
- Google Sheets連携
- インタラクティブCLI実装
