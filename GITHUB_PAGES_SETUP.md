# GitHub Pagesの設定方法

このプロジェクトをGitHub Pagesで公開する手順です。

---

## 前提条件

- GitHubアカウント
- このプロジェクトがGitHubリポジトリにプッシュ済み

---

## 設定手順

### ステップ1: GitHubでリポジトリを開く

ブラウザで自分のリポジトリを開く：
```
https://github.com/yourusername/sales-ops-with-claude
```

---

### ステップ2: Settings を開く

1. リポジトリページの上部メニューで **Settings** をクリック
2. 左サイドバーで **Pages** をクリック

---

### ステップ3: GitHub Pagesを有効化

**Source** セクションで：

1. **Branch** を `main` に設定
2. **Folder** を `/02_apps/whisper-tts-tool/local-whisper-ollama/docs` に設定
3. **Save** をクリック

---

### ステップ4: 公開URLを確認

数分後、ページの上部に以下のメッセージが表示されます：

```
Your site is published at https://yourusername.github.io/sales-ops-with-claude/02_apps/whisper-tts-tool/local-whisper-ollama/docs/
```

このURLでアクセスできます！

---

## カスタムドメインを使う（オプション）

### 独自ドメインの設定

1. **Settings** → **Pages**
2. **Custom domain** に独自ドメインを入力
3. **Save**

DNSの設定が必要です。詳しくは [GitHub Pages公式ドキュメント](https://docs.github.com/ja/pages/configuring-a-custom-domain-for-your-github-pages-site)を参照。

---

## ドキュメント構成

### 公開されるファイル

```
docs/
├── index.md         ← トップページ（メインガイド）
└── _config.yml      ← Jekyll設定
```

### その他のドキュメント（リポジトリ内のみ）

```
local-whisper-ollama/
├── BEGINNER_GUIDE.md       ← ターミナル初心者向け完全ガイド（13KB）⭐
├── START_HERE.md           ← 最初に読むファイル（6.2KB）
├── SETUP_STEP_BY_STEP.md   ← セットアップ詳細（6.3KB）
├── HOW_TO_USE.md           ← 使い方ガイド（11KB）
├── README.md               ← 技術仕様（9.2KB）
├── QUICKSTART.md           ← クイックガイド（3.5KB）
└── USAGE_GUIDE.md          ← 詳しい使い方（5.7KB）
```

---

## ドキュメントの役割分担

| ファイル | 対象 | 内容 |
|---------|------|------|
| **docs/index.md** | Web訪問者 | GitHub Pages用のメインページ |
| **BEGINNER_GUIDE.md** | ターミナル初心者 | 超詳しい手順（画像付き説明） |
| **START_HERE.md** | 初めて使う人 | 全体の案内・ナビゲーション |
| **SETUP_STEP_BY_STEP.md** | セットアップ中の人 | エラー対処含む詳細手順 |
| **HOW_TO_USE.md** | セットアップ完了後 | 使い方・FAQ |
| **README.md** | 開発者 | 技術仕様・詳細 |

---

## READMEにリンクを追加

リポジトリのトップREADMEに以下を追加すると良いです：

```markdown
## 📚 ドキュメント

- [🌐 Web版マニュアル](https://yourusername.github.io/sales-ops-with-claude/02_apps/whisper-tts-tool/local-whisper-ollama/docs/)
- [🔰 ターミナル初心者向けガイド](./02_apps/whisper-tts-tool/local-whisper-ollama/BEGINNER_GUIDE.md)
- [📖 使い方ガイド](./02_apps/whisper-tts-tool/local-whisper-ollama/HOW_TO_USE.md)
```

---

## テーマのカスタマイズ

`docs/_config.yml` を編集してテーマを変更できます：

### 利用可能なテーマ

```yaml
# Cayman（デフォルト、おすすめ）
theme: jekyll-theme-cayman

# その他のテーマ
theme: jekyll-theme-minimal
theme: jekyll-theme-slate
theme: jekyll-theme-architect
```

---

## トラブルシューティング

### ページが表示されない

1. **数分待つ**（初回は5-10分かかることがあります）
2. **Settings → Pages** で URL を確認
3. **ブラウザのキャッシュをクリア**

---

### 404 Not Found

**原因:** パスが間違っている

**解決策:**
```
正しいURL: https://yourusername.github.io/sales-ops-with-claude/02_apps/whisper-tts-tool/local-whisper-ollama/docs/
```

最後の `/docs/` まで必要です。

---

### レイアウトが崩れる

**原因:** `_config.yml` の設定ミス

**解決策:**
```yaml
theme: jekyll-theme-cayman
title: 完全ローカル会議文字起こしツール
description: faster-whisper + Ollama で完全無料の議事録自動生成
```

---

## 更新方法

ドキュメントを更新したら：

```bash
git add docs/
git commit -m "docs: Update documentation"
git push origin main
```

数分後、GitHub Pagesに反映されます。

---

## まとめ

1. **Settings → Pages** で有効化
2. **Branch: main, Folder: /docs** を選択
3. **公開URLでアクセス**

これだけ！
