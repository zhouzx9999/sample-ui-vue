sample-ui-vue
---

### はじめに

[BootStrap](http://getbootstrap.com/) / [Vue.js](http://jp.vuejs.org/) を元にしたプロジェクト Web リソース ( HTML / CSS / JS ) です。 SPA ( Single Page Application ) モデルを前提としています。  

※ライブラリではなく上記ライブラリを用いた単純な実装サンプルです。

サンプル確認用の API サーバとして以下のいずれかを期待します。
- [sample-boot-hibernate](https://github.com/jkazama/sample-boot-hibernate)
- [sample-boot-micro](https://github.com/jkazama/sample-boot-micro)
- [sample-boot-scala](https://github.com/jkazama/sample-boot-scala)
- [sample-aspnet-api](https://github.com/jkazama/sample-aspnet-api)
- [sample-php7-laravel](https://github.com/jkazama/sample-php7-laravel)

---
従来型のマルチページ実装については [sample-ui-vue-pages](https://github.com/jkazama/sample-ui-vue-pages) 、 Flux 実装については [sample-ui-vue-flux](https://github.com/jkazama/sample-ui-vue-flux) を参照してください。

#### ビルド / テスト稼働環境構築

ビルドは [Node.js](http://nodejs.jp/) + [Webpack](https://webpack.github.io/) + [Gulp](http://gulpjs.com/) + [Yarn](https://yarnpkg.com/) or [npm](https://www.npmjs.com/) で行います。以下の手順でインストールしてください。

1. Node.js の[公式サイト](http://nodejs.jp/)からインストーラをダウンロードしてインストール。
1. 「 `npm install -g yarn` 」を実行して Yarn をインストール。
    - Mac ユーザは 「 `sudo npm install -g yarn` 」 で。
1. 「 `npm install -g gulp` 」 を実行して Gulp をインストール。
    - Mac ユーザは 「 `sudo npm install -g gulp` 」 で。
1. コンソールで本ディレクトリ直下へ移動後、「 `yarn` 」を実行して `package.json` 内のライブラリをインストール

### 動作確認

動作確認は以下の手順で行ってください。

1. clone した [sample-boot-hibernate](https://github.com/jkazama/sample-boot-hibernate) を起動する。
    - 起動方法は該当サイトの解説を参照
    - application.yml の `extension.security.auth.enabled` を true にして起動すればログイン機能の確認も可能
1. コンソールで本ディレクトリ直下へ移動し、 「 `gulp` 」 を実行
    - 確認用のブラウザが自動的に起動する。うまく起動しなかったときは 「 http://localhost:3000 」 へアクセス

### 開発の流れ

基本的にテンプレート ( .pug / .scss / .js ( ES6 ) [ .vue ] ) を Web リソース ( .html / .css / .js ) へ Gulp / Webpack でリアルタイム変換させながら開発をしていきます。  
動作確認は Gulp で独自に Web サーバを立ち上げた後、ブラウザ上で行います。  

#### 各種テンプレートファイルの解説

- [Pug](https://github.com/pugjs/pug)
    - HTML を生成するテンプレートツール。公式サイト TOP にある簡素な記法が特徴。
- [Sass (SCSS)](http://sass-lang.com/)
    - CSS 表記を拡張するツール。変数や mixin 、ネスト表記などが利用可能。
- [ES6 with Babel](https://babeljs.io/)
    - ES6 用の Polyfill 。 ES5 でも ES6 風に記述が可能。

#### 各種テンプレートファイルの変更監視 / Web サーバ起動

+ コンソールで本ディレクトリ直下へ移動し、 「 `gulp` 」 を実行

### 配布用ビルドの流れ

配布リソース生成の流れは開発時と同様ですが、監視の必要が無いことと、配布リソースに対する minify や revison の付与などを行う必要があるため、別タスク （ build-prod ） で実行します。

#### 配布用 Web リソースのビルド / リリース

+ コンソールで本ディレクトリ直下へ移動し、 「 `gulp build-prod` 」 を実行
+ `dist` ディレクトリ直下に出力されたファイルをリリース先のディレクトリへコピー

### ポリシー

- JS / CSS の外部ライブラリは npm で管理する
    - jQuery や Bootstrap 等、グローバルスコープの汚染を許容するものはビルド済みリソースをそのまま流用する
- プロジェクト固有の JS は Webpack を利用して生成する
    - グローバルスコープの汚染を許容せずにコンポーネントベースで開発する
- Vue.js の実装アプローチは [vue-hackernews-2.0](https://github.com/vuejs/vue-hackernews-2.0) を参考に

#### ディレクトリ構成

ディレクトリ構成については以下を参照してください。

```
gulpfile.babel.js                    … gulp 実行時に利用されるビルドファイル
package.json                         … npm 関連定義
dist                                 … 配布公開リソース ( 自動生成 )
  css                                … CSS
    - style.css                      … src / css 直下のリソース
  fonts                              … アイコンフォント
  js                                 … JavaScript ( ES5 )
    - bundle.js                      … src / js 直下のリソース
    - vendor.bundle.js               … 外部JSライブラリ
  index.html
src
  css                                … CSS テンプレートファイル ( SCSS )
  html                               … HTML テンプレートファイル ( Pug )
  js
    api                              … API処理 (Vue.js 非依存)
    components                       … SPA で利用されるコンポーネント
    filters                          … フィルタ関数
    platform                         … プロジェクト内JSライブラリ (Vue.js 非依存)
    router                           … SPA ルーティング定義
    views                            … 画面コンポーネント
    - app.js                         … SPA における Entry ファイル
    - App.vue                        … ページ横断的に利用されるコンポーネント
  static                             … 画像等コンパイル不要な静的リソースファイル
```

※ gulp コマンドを実行して変更監視を有効にしておくと、 src 配下のリソースを修正した際にリアルタイムで dist 直下のファイルが更新されていきます。

### 依存ライブラリ

| ライブラリ                    | バージョン   | 用途/追加理由 |
| ------------------------- | -------- | ------------- |
| `vue`                     | 2.5.+    | アプリケーションの MVVM 機能を提供 |
| `vue-router`              | 3.0.+    | Vue.js の SPA ルーティングサポート |
| `jquery`                  | 3.3.+    | DOM 操作サポート |
| `lodash`                  | 4.17.+   | 汎用ユーティリティライブラリ |
| `moment`                  | 2.20.+   | 日時ライブラリ |
| `bootstrap-sass-official` | 3.3.+    | CSS フレームワーク |
| `fontawesome`             | 4.7.+    | フォントアイコンライブラリ |

### License

本サンプルのライセンスはコード含めて全て *MIT License* です。  
プロジェクト立ち上げ時のベース実装サンプルとして気軽にご利用ください。
