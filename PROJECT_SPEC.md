# プロジェクト仕様書：文系出身、中年エンジニアの回り道

作成日: 2026-09-02
このファイルは、端末再起動やセッション断絶時に開発を再開できるように、これまでの経緯と現状をまとめたものです。

---

## 1. プロジェクト概要

- **ブログ名**: 文系出身、中年エンジニアの回り道
- **サイト内での名乗り**: 文中（ぶんちゅう）※「文系中年」の略
- **目的**: 個人の日記ブログ。仕事・育児・趣味などを気ままに記録する場所
- **公開範囲**: 一般公開（ただし検索エンジン未登録の状態。URLを知っている人のみ実質アクセス可能）

### 運営者プロフィール（自己紹介記事・Aboutページの元情報）
- 文系出身のシステムエンジニア（SES業界）、経験8年目
- 4歳と1歳の子供がいる
- 趣味は読書、漫画、映画鑑賞
- 小学校から高校まで野球をやっていた

---

## 2. 技術構成

| 項目 | 内容 |
|---|---|
| フレームワーク | Astro（公式blogテンプレート使用） |
| 記事管理 | Markdown（`src/content/blog/` 配下） |
| コード管理 | GitHub |
| ホスティング | Vercel（GitHub連携で自動デプロイ） |
| ローカルパス | `C:\Users\Prdm011\Desktop\my-diary-blog` |
| GitHubリポジトリ | https://github.com/0830tandy-oss/my-diary-blog |
| GitHubアカウント | 0830tandy-oss |
| 公開URL | https://my-diary-blog-one.vercel.app/ |

### Astroを選んだ理由
- note/はてなブログ（手軽だが自由度低い） と Next.js自作（自由だが日記用途にはオーバースペック）の中間
- Markdownで書くだけでブログになり、コード量が少なく、Vercelで無料公開でき、デザインも自由
- **資産性**が高い：記事データは自分のGitHubリポジトリに残り、プラットフォーム終了リスクがない。SEO評価も独自ドメインに蓄積される（note/はてなブログはプラットフォームのドメインに評価が乗る）

---

## 3. 現在の設定値（2026-09-02時点）

**`src/consts.ts`**
```ts
export const SITE_TITLE = '文系出身、中年エンジニアの回り道';
export const SITE_DESCRIPTION = '文系出身、エンジニア8年目。仕事や育児、読書や野球の話を綴る日記です。';
```

**`astro.config.mjs`**
```js
site: 'https://my-diary-blog-one.vercel.app',
```
※以前 `https://example.com`（テンプレートの初期値）のままになっていた不具合を修正済み。sitemap-index.xml のURLが正しく生成されるようになった。

**`src/components/Footer.astro`**
- コピーライト表記を `© {year} 文中. All rights reserved.` に変更済み

**`src/pages/about.astro`**
- プロフィール内容に書き換え済み（Lorem ipsumから置換）

---

## 4. 公開済みコンテンツ

- テンプレートのサンプル記事5件（first-post, second-post, third-post, using-mdx, markdown-style-guide）は削除済み
- 初回記事「はじめまして」（`src/content/blog/2026-08-29-introduction.md`）を公開済み
  - 自己紹介、ブログを始めたきっかけ（YouTubeでの田中渓さん・勝間和代さんの対談動画がきっかけの一つ、AIの進化がもう一つ）を記載

---

## 5. 運用フロー（記事の書き方〜公開まで）

1. `C:\Users\Prdm011\Desktop\my-diary-blog\src\content\blog\` にMarkdownファイルを追加/編集
2. ターミナルで以下を実行：
   ```
   cd "C:\Users\Prdm011\Desktop\my-diary-blog"
   npm run build   # ビルド確認
   git add -A
   git commit -m "コミットメッセージ"
   git push
   ```
3. pushすると自動的にVercelが再デプロイ（1分程度で反映）

### 記事のfrontmatter形式
```md
---
title: 'タイトル'
description: '説明文'
pubDate: 2026-08-29
---

本文...
```

---

## 6. 認証・アカウント状況

- **GitHub CLI (`gh`)**: ローカル端末で認証済み（アカウント: 0830tandy-oss）。`gh auth status` で確認可能
- **Vercel**: GitHubアカウントでサインアップ・連携済み。リポジトリインポート＆デプロイ設定済み
- **2段階認証（2FA）**: Vercel側で設定を推奨した状態（設定完了したかは未確認）

---

## 7. 新PC移行時の手順

GitHubにコードを保存しているため、以下の手順で環境を復元可能：
1. Node.jsをインストール
2. `git clone https://github.com/0830tandy-oss/my-diary-blog.git`
3. `cd my-diary-blog && npm install`
4. `npm run dev`（ローカル確認）／ `git push`（公開反映）
5. GitHub CLIも必要なら `winget install --id GitHub.cli -e` → `gh auth login --web`

---

## 8. 保留中・今後のタスク

### Google Search Console登録（未着手）
検索エンジンに載せたい場合の作業。以下の手順で進める予定：
1. https://search.google.com/search-console でプロパティ追加（URLプレフィックス方式、`https://my-diary-blog-one.vercel.app`）
2. 所有権確認は「HTMLタグ」方式を選択し、発行されたmetaタグを取得
3. metaタグを `src/layouts/BlogPost.astro` または `src/components/BaseHead.astro` の `<head>` に追加
4. ビルド確認 → git commit/push → Vercel反映
5. Search Console側でサイトマップ（`/sitemap-index.xml`）を送信

**リマインド設定済み**: 2026-09-03(木) 21:00（日本時間）に、claude.aiのルーティン機能でリマインドメッセージが届くよう設定済み（routine ID: `trig_019Mu6ZDexyLTR5rUBrDys51`）

### note併用（方針決定済み、実施は未着手）
- 目的：アクセス数（拡散力）を補うため
- **方針**：「noteは入口、ブログ（Astro）は本編」で進めることに決定（2026-09-07）
  - noteには人柄やブログを始めたきっかけなど導入的な内容を書く
  - 詳細な本文はAstro側で読んでもらう形にし、noteの記事末尾などに「続きはこちら」でAstro記事へ誘導するリンクを設置
  - 全文を両方に載せると重複コンテンツとしてSEO評価が分散するリスクがあるため、この使い分けを採用
  - 実際に同様の運用をしている個人ブロガーの実例も複数確認済み
- 未着手。noteは公式の投稿API連携がないため、投稿は手動作業が前提

### サンプル記事の扱い
- 削除済み（2026-08-29に対応）

---

## 9. 参考：プラットフォーム比較の結論（過去の相談内容）

| | Astro（現状） | note | はてなブログ |
|---|---|---|---|
| アクセス数（拡散力） | 低い（発見の仕組みなし） | 高い（おすすめ機能・読者コミュニティ） | 中〜高（はてなブックマークでバズる可能性） |
| 資産性（データ所有・SEO蓄積） | 高い（自分のリポジトリ・独自ドメイン） | 低い（プラットフォーム依存） | 低い（プラットフォーム依存） |
| カスタマイズ自由度 | 高い | 低い | 中 |
| 移行のしやすさ | 元がMarkdownなので移行しやすい | - | MovableType形式でインポート可能 |

**方針**: 現状は資産性を重視してAstroで運用。将来的にnoteとの併用（B案）を検討中。

---

## 10. よくあるトラブルと対処

- **記事削除後にビルドエラーが出る場合**：Astroのcontent collectionキャッシュが残っていることが原因。以下でキャッシュをクリアしてから再ビルド：
  ```
  rm -rf .astro dist node_modules/.astro node_modules/.vite
  npm run build
  ```
