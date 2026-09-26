# 公開手順（GitHub Pages）

アプリは `index.html` の1ファイルだけ。GitHub Pages で公開し、スマホのブラウザでURLを開いて使う。

- リポジトリ：`floor-layout`（公開リポジトリ）
- 公開URL：`https://<ユーザー名>.github.io/floor-layout/`

> リポジトリは誰でも見られる。職場名・実際の職場の間取りなどのデータは入れない。
> 間取りのデータはスマホのブラウザ内（localStorage）にだけ保存され、リポジトリには入らない。

---

## 初回の公開（済み）

パソコンで Claude Code に「公開して」と頼めば、下の作業をまとめて行う。

1. GitHub CLI を入れる：`winget install --id GitHub.cli -e`
2. ログイン：ターミナルで `gh auth login` を実行し、画面の質問に答える
   - `Where do you use GitHub?` → `GitHub.com`
   - `What is your preferred protocol for Git operations on this host?` → `HTTPS`
   - `Authenticate Git with your GitHub credentials?` → `Yes`
   - `How would you like to authenticate GitHub CLI?` → `Login with a web browser`
   - 表示された8桁のコードを控えて Enter → ブラウザで開いた GitHub の画面にコードを入力 → `Continue` → `Authorize github`
3. リポジトリ作成と送信：
   ```
   git init -b main
   git add index.html PUBLISH.md .gitignore
   git commit -m "初回公開"
   gh repo create floor-layout --public --source . --push
   ```
4. GitHub Pages を有効にする：
   ```
   gh api -X POST repos/{owner}/floor-layout/pages -f "source[branch]=main" -f "source[path]=/"
   ```
   画面で行う場合：リポジトリのページ → `Settings` → 左の `Pages` →
   `Build and deployment` の `Source` を `Deploy from a branch` →
   `Branch` を `main`・`/ (root)` にして `Save`
5. 1〜2分待つと公開URLで開けるようになる

## 更新を反映するとき（2回目以降）

`index.html` を直したあと、`floor-layout` フォルダで：

```
git add index.html
git commit -m "変更内容を短く"
git push
```

1〜2分で公開URLに反映される。スマホで古い画面のままなら、ページを再読み込みする
（iPhone の Safari はアドレスバー左の `ぁあ` → `Webサイトを再読み込み`、Android の Chrome は右上の `⋮` → 更新アイコン）。

反映状況の確認：リポジトリのページ → `Actions` タブ → 一番上の `pages build and deployment` が緑のチェックなら完了。

## スマホでの使い方のコツ

- 公開URLを開き、ホーム画面に追加するとアプリのように使える
  - iPhone（Safari）：下の共有ボタン → `ホーム画面に追加`
  - Android（Chrome）：右上の `⋮` → `ホーム画面に追加`
- データはそのスマホのそのブラウザにだけ保存される。ブラウザの「履歴とWebサイトデータを消去」をすると消えるので注意（フェーズ3でバックアップ機能を追加予定）
