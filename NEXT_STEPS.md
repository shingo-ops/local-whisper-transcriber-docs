# 🚨 次回作業再開時の手順

**作成日時**: 2026年3月13日 14:55
**問題**: GitHub Pagesで404エラーが表示される

---

## 📋 クイックスタート（次回再開時）

### ステップ1: 現在の状況確認（30秒）

```bash
cd ~/local-whisper-transcriber-docs

# GitHub Pagesのビルド状況確認
gh api repos/shingo-ops/local-whisper-transcriber-docs/pages

# ステータスが "built" になっているか確認
```

### ステップ2: ブラウザでアクセス（30秒）

```bash
# キャッシュをクリアしてアクセス（Cmd + Shift + R）
open https://shingo-ops.github.io/local-whisper-transcriber-docs/
```

**✅ 表示された場合**: 問題解決！このファイルは削除してOK

**❌ まだ404の場合**: ステップ3へ

---

### ステップ3: ローカルで動作確認（1分）

```bash
cd ~/local-whisper-transcriber-docs

# ローカルサーバー起動
npx docsify-cli serve .
```

ブラウザで http://localhost:3000 を開く

**✅ ローカルで表示された場合**:
- Docsify設定は正しい
- GitHub Pagesの問題 → ステップ4へ

**❌ ローカルでも表示されない場合**:
- Docsify設定に問題 → ステップ5へ

---

### ステップ4: GitHub Pages再設定（2分）

```bash
cd ~/local-whisper-transcriber-docs

# GitHub Pages一旦無効化
gh api repos/shingo-ops/local-whisper-transcriber-docs/pages -X DELETE

# 30秒待機
sleep 30

# 再有効化
gh api repos/shingo-ops/local-whisper-transcriber-docs/pages -X POST -f 'source[branch]=main' -f 'source[path]=/'

# さらに1分待機してからアクセス
sleep 60
open https://shingo-ops.github.io/local-whisper-transcriber-docs/
```

---

### ステップ5: シンプル構成に変更（3分）

Docsifyを諦めて、GitHub標準のmarkdown表示に切り替え:

```bash
cd ~/local-whisper-transcriber-docs

# ルートにREADME.md配置
cp docs/README.md README.md

# コミット＆プッシュ
git add README.md
git commit -m "fix: ルートにREADME.md配置（GitHub Pages対応）"
git push origin main

# 1分待機してアクセス
sleep 60
open https://shingo-ops.github.io/local-whisper-transcriber-docs/
```

この方法だと、Docsifyの機能（検索、サイドバーなど）は使えませんが、最低限ドキュメントは表示されます。

---

## 📊 各方法の比較

| 方法 | 所要時間 | 検索機能 | サイドバー | 見た目 |
|------|---------|---------|-----------|--------|
| **Docsify（現在）** | - | ✅ | ✅ | ⭐⭐⭐⭐⭐ |
| **シンプルMarkdown** | 3分 | ❌ | ❌ | ⭐⭐⭐ |

---

## 🔗 参考情報

- **詳細な記録**: `PROJECT_STATUS.md`
- **リポジトリ**: https://github.com/shingo-ops/local-whisper-transcriber-docs
- **目標URL**: https://shingo-ops.github.io/local-whisper-transcriber-docs/
- **ローカルディレクトリ**: ~/local-whisper-transcriber-docs

---

## 💡 その他のヒント

### ブラウザのキャッシュクリア方法

- **Chrome/Edge**: Cmd + Shift + R（ハードリロード）
- **Safari**: Cmd + Option + E（キャッシュ空に）→ Cmd + R
- **Firefox**: Cmd + Shift + R

### GitHub Pagesのビルドログ確認

```bash
# GitHub Actionsのワークフロー確認
open https://github.com/shingo-ops/local-whisper-transcriber-docs/actions
```

---

**作業再開時は、まずステップ1から順に試してください！**
