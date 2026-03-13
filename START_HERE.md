# 🎤 完全ローカル会議文字起こしツール

**faster-whisper + Ollama で完全無料の議事録自動生成**

---

## 📚 ドキュメント一覧

### 🚀 まず読むべきファイル

| ファイル | 内容 | 誰向け |
|---------|------|--------|
| **[SETUP_STEP_BY_STEP.md](./SETUP_STEP_BY_STEP.md)** | **実践的なセットアップガイド**<br>今すぐ始める人向け。エラー対処法も含む | 初めて使う人 |
| **[HOW_TO_USE.md](./HOW_TO_USE.md)** | **使い方ガイド**<br>セットアップ後の操作方法 | セットアップ完了後 |

### 📖 その他のドキュメント

| ファイル | 内容 |
|---------|------|
| [README.md](./README.md) | 詳細な技術仕様とリファレンス |
| [QUICKSTART.md](./QUICKSTART.md) | 5分で始めるクイックガイド |
| [USAGE_GUIDE.md](./USAGE_GUIDE.md) | より詳しい使い方とFAQ |

---

## ⚡ クイックスタート

### 初めて使う場合（初回のみ）

```bash
# 1. ディレクトリに移動
cd ~/sales-ops-with-claude/02_apps/whisper-tts-tool/local-whisper-ollama

# 2. セットアップ（初回のみ）
./setup.sh
```

**所要時間: 15-30分**（モデルのダウンロード含む）

詳しくは → **[SETUP_STEP_BY_STEP.md](./SETUP_STEP_BY_STEP.md)**

---

### 2回目以降（毎回）

```bash
# ディレクトリに移動して起動
cd ~/sales-ops-with-claude/02_apps/whisper-tts-tool/local-whisper-ollama
./run.sh
```

または、エイリアスを設定すれば：

```bash
meeting
```

これだけ！

詳しくは → **[HOW_TO_USE.md](./HOW_TO_USE.md)**

---

## 💡 主な機能

### ✅ 完全ローカル・無料
- APIコスト **0円**
- プライバシー完全保護
- オフライン動作可能

### ✅ リアルタイム録音
- マイクから直接録音
- 自動で文字起こし + 議事録生成
- 30分会議 → 約10分で完了

### ✅ 既存ファイル処理
- mp3, m4a, wav, webm 対応
- ドラッグ&ドロップで簡単

### ✅ Google Sheets連携
- 既存のOpenAI Whisperアプリと併用
- ハイブリッド運用で65%コスト削減

### ✅ RAG機能
- 専門用語・組織情報を自動参照
- 精度が30-50%向上

---

## 📊 性能・品質

### コスト比較（30分会議）

| 処理 | OpenAI API | **本ツール** |
|------|-----------|------------|
| 文字起こし | 約27円 | **0円** |
| 議事録生成 | 約50円 | **0円** |
| **合計** | **約77円** | **0円** |

### 処理時間（30分会議）

| 処理 | 時間 |
|------|------|
| 文字起こし | 約10分 |
| 議事録生成 | 約30秒 |
| **合計** | **約10分30秒** |

### 品質

| 項目 | 評価 |
|------|------|
| 文字起こし精度 | ★★★★☆ |
| 議事録品質 | ★★★★☆ |
| 専門用語対応（RAG使用時） | ★★★★★ |

---

## 🎯 使い方の流れ

### パターン1: リアルタイム録音

```
1. ./run.sh で起動
   ↓
2. メニューで「2」を選択
   ↓
3. 録音開始（Enter押す）
   ↓
4. 会議中...
   ↓
5. 会議終了（Ctrl+C で停止）
   ↓
6. 自動処理（文字起こし + 議事録）
   ↓
7. 完了！output/フォルダに保存
```

### パターン2: 既存ファイル処理

```
1. ./run.sh で起動
   ↓
2. メニューで「1」を選択
   ↓
3. ファイルパス入力（ドラッグ&ドロップ可）
   ↓
4. 自動処理（文字起こし + 議事録）
   ↓
5. 完了！output/フォルダに保存
```

---

## 📦 必要なもの

### ハードウェア
- **Mac**（Apple Silicon または Intel）
- **メモリ**: 8GB以上推奨
- **ディスク**: 10GB以上の空き

### ソフトウェア
- **macOS**
- **ターミナル**
- **インターネット**（初回セットアップのみ）

すべて自動でインストールされます！

---

## 🆘 困ったら

### よくあるエラー

#### `source: no such file or directory: venv/bin/activate`

**原因:** まだセットアップが完了していない

**解決策:**
```bash
./setup.sh
```

---

#### `zsh: command not found: python`

**原因:** macOSでは `python3` を使う

**解決策:**
```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

---

#### `ollama: command not found`

**原因:** Ollamaがインストールされていない

**解決策:**
```bash
brew install ollama
ollama serve &
```

---

詳しくは → **[SETUP_STEP_BY_STEP.md](./SETUP_STEP_BY_STEP.md)** のトラブルシューティング

---

## 🗂️ プロジェクト構造

```
local-whisper-ollama/
├── START_HERE.md           ← 📌 このファイル（最初に読む）
├── SETUP_STEP_BY_STEP.md   ← 📌 セットアップガイド
├── HOW_TO_USE.md           ← 📌 使い方ガイド
├── README.md               ← 詳細ドキュメント
├── QUICKSTART.md           ← クイックガイド
├── USAGE_GUIDE.md          ← 詳しい使い方
│
├── setup.sh                ← セットアップスクリプト
├── run.sh                  ← 起動スクリプト
├── main.py                 ← メインアプリ
│
├── src/                    ← ソースコード
│   ├── whisper_transcriber.py
│   ├── ollama_processor.py
│   ├── rag_engine.py
│   └── sheets_integration.py
│
├── knowledge_base/         ← RAG用ナレッジベース
│   ├── sample_terms.md
│   └── sample_members.md
│
├── audio_files/            ← 録音ファイル保存先
└── output/                 ← 処理結果保存先
```

---

## 📝 次のアクション

### ステップ1: セットアップ

まだセットアップしていない場合：

👉 **[SETUP_STEP_BY_STEP.md](./SETUP_STEP_BY_STEP.md)** を読む

### ステップ2: 使ってみる

セットアップ完了後：

👉 **[HOW_TO_USE.md](./HOW_TO_USE.md)** を読む

### ステップ3: カスタマイズ

基本的な使い方に慣れたら：

- RAGナレッジベースに専門用語を追加
- Google Sheets連携を設定
- プロンプトをカスタマイズ

---

## 🎓 ライセンス

MIT License

---

## 👤 作成者

谷澤真吾

---

**今すぐ始める → [SETUP_STEP_BY_STEP.md](./SETUP_STEP_BY_STEP.md)**
