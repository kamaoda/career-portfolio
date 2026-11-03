# 転職活動用の個人サイト

職務経験、技術的な判断、ソフトウェア開発に対する考え方を公開するための雛形です。コンテンツはMarkdownで管理し、GitHub PagesとJekyllで静的HTMLに変換します。本文の取得・閲覧にJavaScriptは必要ありません。

テーマはGitHub Pages公式対応の[Minimal](https://github.com/pages-themes/minimal)です。Next.js、React、npm、独自ビルドツールやGitHub Actionsのワークフローは使用しません。

## ファイル構成

```text
AGENTS.md                 Codexの作業ルール
README.md                 リポジトリの説明と公開手順
Gemfile / Gemfile.lock    ローカル検証用の依存関係
.gitignore                ローカル環境・生成物の除外
_config.local.yml         ローカル専用の設定（公開対象外）
docs/
  _config.yml             Jekyll設定
  index.md                トップページ
  about.md                職務経歴・技術スタック
  cases/index.md          実績一覧
  cases/daily-batch-validation.md  日次バッチ検証のケーススタディ
  cases/insurance-contract-integration.md  外部連携バッチのケーススタディ
  cases/attendance-automation.md  勤怠集計自動化のケーススタディ
  assets/images/daily-batch-validation-flow.png  提供された業務フロー比較図
  assets/images/insurance-contract-integration-*.png  外部連携バッチの概念図2枚
  assets/images/attendance-*.svg  勤怠集計の業務フロー・Excel設計構成図
  assets/css/case-study.css  詳細ページ専用の表示調整
  _layouts/case-study.html 詳細ページ用レイアウト（Minimalを継承）
  _includes/head-custom.html  詳細ページ専用CSSの読み込み
  principles/index.md     開発に対する考え方一覧
```

## コンテンツの編集

各ページの［ ］内を、本人が確認した公開可能な情報に置き換えます。雛形には職務経験・成果・数値・技術スタックの具体例を含めていません。守秘義務に関わる情報や第三者の非公開情報は記載しないでください。

ページ先頭のYAML front matter（`---`で囲まれた部分）を保持してください。`title`はページのタイトル、`permalink`は公開URLのパスです。本文は見出しと文章を中心に記述し、リンクには内容が分かる名前を付けます。

内部リンクは、公開パスを補うJekyllの[`relative_url`](https://jekyllrb.com/docs/liquid/filters/)フィルターを使用します。例:

```liquid
[職務経歴・技術スタック]({{ '/about/' | relative_url }})
```

## 公開前の設定

このリポジトリの公開URLは https://kamaoda.github.io/career-portfolio/ です。公開元は `main` ブランチの `docs/`、`url` は `https://kamaoda.github.io`、`baseurl` は `/career-portfolio` に設定しています。

`docs/_config.yml`の`url`と`baseurl`を実際の公開先に合わせます。以下の`USERNAME`と`REPOSITORY`は説明用のプレースホルダーです。

| 公開先 | `url` | `baseurl` |
| --- | --- | --- |
| プロジェクトサイト | `https://USERNAME.github.io` | `/REPOSITORY` |
| ユーザーサイト（`USERNAME.github.io`リポジトリ） | `https://USERNAME.github.io` | `""` |
| 独自ドメイン | `https://実際のドメイン` | `""` |

`url`と`baseurl`の末尾に`/`は付けません。独自ドメインを使用する場合はGitHub Pages側でも別途設定が必要です。

## 公開手順（明示的に公開を指示した後に実施）

GitHubへのpushとサイト公開は別の操作です。GitHub Pagesの有効化・公開は、明示的な指示を受けた後に実施します。

1. `main`から作成したローカルの`codex/作業内容`ブランチで変更し、作業ブランチはpushしません。リリースの指示を受けたら、本文・画像・設定などの差分全体を点検し、公開可能な変更を`git merge --squash`で`main`へ取り込んで1件の公開用コミットにまとめます。既存の未pushコミットがある場合は、それらも公開対象として確認します。GitHubへのpushは、pushの明示的な指示を受けてから実施します。
2. GitHubリポジトリの **Settings → Pages → Build and deployment** を開きます。
3. **Source**を **Deploy from a branch** に設定します。
4. **Branch**を **main**、フォルダーを **/docs** に設定して **Save** します。
5. ビルド完了後、Pages設定画面に表示された公開URLを開きます。

公開元は **mainブランチのdocs/** です。有効化後は公開元へのpushにより自動で再公開されます。`.nojekyll`は追加しないでください。

詳細は[GitHub公式の公開元設定手順](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site)を参照してください。

ブランチ運用の詳細は`AGENTS.md`に記載しています。squashは途中のコミット履歴をmainに持ち込まないための操作であり、最終ファイルから非公開情報を除去するものではありません。機密情報や非公開の原稿・素材はGitに記録しないでください。

## 確認方法

公開前には、各ページのfront matter、見出し、内部リンク、および`url`・`baseurl`を確認します。GitHub上のMarkdownプレビューではLiquidリンクは変換されないため、リンク動作の確認にはJekyllのビルド結果を使用します。

### ローカル環境の準備（Ubuntu / WSL）

Ruby 3.3とBundlerを使用します。`Gemfile`で[GitHub Pagesの依存バージョン一覧](https://pages.github.com/versions/)に合わせた`github-pages` gemと、プレビュー用の`webrick`を指定し、`Gemfile.lock`で依存関係を固定します。[GitHub公式のローカル確認手順](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll)に沿った構成です。

この作業環境には、管理者権限を使わずにプロジェクト専用のRubyを`.local/`へ導入しています。WSLのターミナルでリポジトリのルートを開き、ターミナルを開くたびに以下を実行します。

```sh
source .local/env
ruby --version
bundle --version
```

`.local/`はGit管理対象外です。別のPCへクローンする場合はRubyとBundlerを別途インストールしてください。Ubuntu 24.04 / WSLでシステムのRubyを使う場合の例は以下です（sudoパスワードが必要です）。

```sh
sudo apt-get update
sudo apt-get install -y ruby-full ruby-bundler build-essential zlib1g-dev libffi-dev
```

Ruby 3.2系を使う場合も、以下の依存関係のインストールとビルドで互換性を確認してください。

### 依存関係のインストール

リポジトリのルートで実行します。gemはプロジェクト内に保存します。初回と`Gemfile.lock`変更後に実行してください。

```sh
bundle config set --local path vendor/bundle
bundle install
```

`Gemfile.lock`は共有し、`.local/`、`.bundle/`、`vendor/bundle/`と生成HTMLはコミットしません。

### ビルドとプレビュー

`_config.local.yml`を重ねて、ローカルではテーマのGitHubリポジトリ情報の参照を抑えます。GitHubの認証情報は不要です。WSLのWindowsドライブ上での編集検知に対応するため、ローカルでは`force_polling: true`で定期的にファイル変更を確認します。このファイルは`docs/`の外にあるため公開時には使用されません。

```sh
bundle exec jekyll build --source docs --destination _site --config docs/_config.yml,_config.local.yml --strict_front_matter
bundle exec jekyll serve --source docs --destination _site --config docs/_config.yml,_config.local.yml --host 127.0.0.1 --baseurl ""
```

ブラウザーで`http://localhost:4000/`を開きます。Markdownを保存すると自動で再ビルドされるので、ブラウザーを再読み込みして確認します。停止は`Ctrl+C`です。`_config.yml`を変更した場合はサーバーを再起動します。

`--baseurl ""`はローカル起動時だけ公開パスを空にします。公開設定の`docs/_config.yml`は書き換えません。公開先と同じパスを確認する場合は、サーバーを停止して以下を実行します。

```sh
bundle exec jekyll serve --source docs --destination _site --config docs/_config.yml,_config.local.yml --host 127.0.0.1 --baseurl /career-portfolio
```

`http://localhost:4000/career-portfolio/`から各ページのナビゲーションを確認します。`/career-portfolio`は実際のリポジトリ名に合わせて変更してください。

公開後は、次のURL（プロジェクトサイトの例）から相互に移動できることを確認します。

- `https://USERNAME.github.io/REPOSITORY/`
- `https://USERNAME.github.io/REPOSITORY/about/`
- `https://USERNAME.github.io/REPOSITORY/cases/`
- `https://USERNAME.github.io/REPOSITORY/cases/daily-batch-validation/`
- `https://USERNAME.github.io/REPOSITORY/cases/insurance-contract-integration/`
- `https://USERNAME.github.io/REPOSITORY/cases/attendance-automation/`
- `https://USERNAME.github.io/REPOSITORY/principles/`

ブラウザーの「ページのソースを表示」、または`curl -L <ページURL>`で、各ページの見出しと本文がHTMLに含まれることも確認してください。ローカルの生成物では`_site/index.html`、`_site/about/index.html`、`_site/cases/index.html`、`_site/cases/daily-batch-validation/index.html`、`_site/cases/insurance-contract-integration/index.html`、`_site/cases/attendance-automation/index.html`、`_site/principles/index.html`が対象です。

### ケーススタディの確認

事例①は`/cases/daily-batch-validation/`、事例②は`/cases/insurance-contract-integration/`、事例③は`/cases/attendance-automation/`です。`codex-input/`の提供原稿とPNG・SVGをもとに、本文と画像を`docs/`へ配置しています。入力ファイル自体は公開元の外にあり、`codex-input/`はGit管理対象外としてローカルに保持します。掲載内容は`docs/`で管理します。

- トップ・実績一覧から3件の詳細へ移動し、末尾のリンクで一覧へ戻れることを確認します。
- 事例①は概要の後に比較図、事例②は「設計方針」にBefore / After図、「ローカル自動テスト」に構成図が表示されることを確認します。図または各図の「原寸で開く・拡大する」リンクから元の画像を開けることを確認します。小さな画面では開いた画像をブラウザーで拡大できます。
- 事例③は「従来の業務フロー」の説明直後に比較図、「全社員分を一括計算するExcelを設計」に構成図をSVGで表示します。図内の製品名は公開原稿の匿名化方針に合わせて「勤怠管理SaaS」に一般化しています。図と拡大リンクからSVGを開き、日本語・記号・翌月への残高引き継ぎを確認します。
- PCとスマートフォンの幅で、見出し・表・画像が横にはみ出さず、画像の縦横比が保たれることを確認します。
- `baseurl`が空の場合と`/career-portfolio`の場合の両方で、一覧・詳細・画像・CSSへのリンクを確認します。

詳細ページだけに専用CSSを読み込み、既存ページにはMinimalテーマのスタイルをそのまま適用します。公開方式は引き続きmainブランチの`docs/`です。
