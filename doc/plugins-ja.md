![rehype][logo]

# プラグイン

**rehype**はプラグインを使用してHTMLを変換するツールです。
rehypeエコシステムに関する情報は[モノレポのREADME][rehype]をご覧ください。
このページでは既存のプラグインを紹介します。

## 目次

* [プラグインリスト](#プラグインリスト)
* [ユーティリティリスト](#ユーティリティリスト)
* [プラグインの使用](#プラグインの使用)
* [プラグインの作成](#プラグインの作成)

## プラグインリスト

エコシステム内の最も素晴らしいプロジェクトについては[`awesome-rehype`][awesome-rehype]をご覧ください。
より多くのプラグインは、GitHub上で[`rehype-plugin`トピック][topic]でタグ付けされています。

> 💡 **ヒント**: rehypeプラグインはHTMLで動作し、**remark**プラグインはマークダウンで動作します。
> より多くのプラグインについては、remarkの[プラグインリスト][remark-plugins]をご覧ください。

プラグインリスト:

* [`rehype-accessible-emojis`](https://github.com/GaiAma/Coding4GaiAma/tree/HEAD/packages/rehype-accessible-emojis)
  — role & aria-labelを追加して絵文字をアクセシブルにする
* [`rehype-annotate`](https://github.com/baldurbjarnason/rehype-annotate)
  — W3Cスタイルのアノテーションを追加
* [`rehype-attr`](https://github.com/jaywcjlove/rehype-attr)
  — 属性を追加するための新しいマークダウン構文
* [`rehype-auto-ads`](https://github.com/robot-Inventor/rehype-auto-ads)
  — 指定された段落数ごとに広告コードを挿入
* [`rehype-autolink-headings`](https://github.com/rehypejs/rehype-autolink-headings)
  — 見出しにリンクを追加
* [`rehype-callouts`](https://github.com/lin-stephanie/rehype-callouts)
  — ブロッククォートベースのコールアウト（admonitions/alerts）をレンダリング
* [`rehype-citation`](https://github.com/timlrx/rehype-citation)
  — bibtexから引用と参考文献を追加
* [`rehype-class-names`](https://github.com/riderjensen/rehype-class-names)
  — セレクタによってクラスを追加
* [`rehype-color-chips`](https://github.com/shreshthmohan/rehype-color-chips)
  — カラーコードを含むインラインコードブロックにカラーチップを追加
* [`rehype-components`](https://github.com/marekweb/rehype-components)
  — コンポーネント（カスタム要素）をレンダリング
* [`rehype-concat-css-style`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-concat-css-style)
  — `<style>`を連結
* [`rehype-concat-javascript`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-concat-javascript)
  — `<script>`を連結
* [`rehype-css-to-top`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-css-to-top)
  — `<link>`を`<head>`に移動
* [`rehype-document`](https://github.com/rehypejs/rehype-document)
  — ドキュメントでラップ
* [`rehype-dom-parse`](https://github.com/rehypejs/rehype-dom/tree/main/packages/rehype-dom-parse)
  — ブラウザでのHTML入力解析のサポートを追加
* [`rehype-dom-stringify`](https://github.com/rehypejs/rehype-dom/tree/main/packages/rehype-dom-stringify)
  — ブラウザでのHTMLシリアル化のサポートを追加
* [`rehype-external-links`](https://github.com/rehypejs/rehype-external-links)
  — 外部リンクにrel（およびtarget）を追加
* [`rehype-extract-meta`](https://github.com/gorango/rehype-extract-meta)
  — HTMLドキュメントからメタデータを抽出
* [`rehype-figure`](https://github.com/josestg/rehype-figure)
  — 画像からfigureとcaptionをサポート
* [`rehype-format`](https://github.com/rehypejs/rehype-format)
  — HTMLをフォーマット
* [`rehype-highlight`](https://github.com/rehypejs/rehype-highlight)
  — `lowlight`を介してHighlight.jsでコードブロックの構文をハイライト
* [`rehype-highlight-code-block`](https://github.com/mapbox/rehype-highlight-code-block)
  — 提供する任意の関数でコードブロックの構文をハイライト
* [`rehype-highlight-code-lines`](https://github.com/ipikuka/rehype-highlight-code-lines)
  — コードブロックに行番号を追加; 希望するコード行のハイライトを許可
* [`rehype-infer-description-meta`](https://github.com/rehypejs/rehype-infer-description-meta)
  — ドキュメントの内容からファイルメタデータを推論
* [`rehype-infer-reading-time-meta`](https://github.com/rehypejs/rehype-infer-reading-time-meta)
  — ドキュメントから読み取り時間をファイルメタデータとして推論
* [`rehype-infer-title-meta`](https://github.com/rehypejs/rehype-infer-title-meta)
  — ドキュメントのメインタイトルからファイルメタデータを推論
* [`rehype-inline`](https://github.com/marko-knoebl/rehype-inline)
  — JS、CSS、および画像ファイルをインライン化
* [`rehype-inline-svg`](https://github.com/JS-DevTools/rehype-inline-svg)
  — SVG画像をインライン化および最適化
* [`rehype-ignore`](https://github.com/jaywcjlove/rehype-ignore)
  — HTMLコメントを介してコンテンツの表示を無視
* [`rehype-jargon`](https://github.com/freesewing/freesewing/tree/develop/packages/rehype-jargon)
  — 専門用語の定義を挿入
* [`rehype-javascript-to-bottom`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-javascript-to-bottom)
  — `<script>`を`<body>`の末尾に移動
* [`rehype-join-line`](https://github.com/unix/rehype-join-line)
  — 中国語の段落の改行を解決
* [`rehype-katex`](https://github.com/remarkjs/remark-math/tree/main/packages/rehype-katex)
  — KaTeXで数式をレンダリング
* [`rehype-lodash-template`](https://github.com/viktor-yakubiv/rehype-lodash-template)
  — テンプレート文字列を辞書の値で置換
* [`rehype-mathjax`](https://github.com/remarkjs/remark-math/tree/main/packages/rehype-mathjax)
  — MathJaxで数式をレンダリング
* [`rehype-mermaidjs`](https://github.com/remcohaszing/rehype-mermaidjs)
  — mermaidダイアグラムをレンダリング
* [`rehype-meta`](https://github.com/rehypejs/rehype-meta)
  — ドキュメントのheadにメタデータを追加
* [`rehype-minify`](https://github.com/rehypejs/rehype-minify)
  — HTMLを最小化
* [`rehype-minify-attribute-whitespace`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-minify-attribute-whitespace)
  — 属性の空白を最小化
* [`rehype-minify-css-style`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-minify-css-style)
  — `<style>`内のCSSを最小化
* [`rehype-minify-enumerated-attribute`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-minify-enumerated-attribute)
  — 列挙型属性を最小化
* [`rehype-minify-event-handler`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-minify-event-handler)
  — イベントハンドラ属性を最小化
* [`rehype-minify-javascript-script`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-minify-javascript-script)
  — `<script>`内のJSを最小化
* [`rehype-minify-javascript-url`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-minify-javascript-url)
  — `javascript:` URLを最小化
* [`rehype-minify-json-script`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-minify-json-script)
  — `<script>`内のJSONを最小化
* [`rehype-minify-language`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-minify-language)
  — `lang`属性を最小化
* [`rehype-minify-media-attribute`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-minify-media-attribute)
  — `media`属性を最小化
* [`rehype-minify-meta-color`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-minify-meta-color)
  — テーマカラー`<meta>`の`content`を最小化
* [`rehype-minify-meta-content`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-minify-meta-content)
  — `<meta>`の`content`を最小化
* [`rehype-minify-style-attribute`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-minify-style-attribute)
  — `style`属性を最小化
* [`rehype-minify-url`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-minify-url)
  — URL属性を最小化
* [`rehype-minify-whitespace`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-minify-whitespace)
  — 要素間の空白を最小化
* [`rehype-normalize-attribute-value-case`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-normalize-attribute-value-case)
  — 属性値の大文字小文字を正規化
* [`rehype-partials`](https://github.com/mrzmmr/rehype-partials)
  — rehypeのパーシャルサポート
* [`rehype-picture`](https://github.com/rehypejs/rehype-picture)
  — 画像を`<picture>`でラップ
* [`rehype-postcss`](https://github.com/viktor-yakubiv/rehype-postcss)
  — `<style>`ノードと`style`属性を持つ要素でPostCSSを実行
* [`rehype-prevent-favicon-request`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-prevent-favicon-request)
  — 空の`favicon.ico`を設定してリクエストを防止
* [`rehype-prism`](https://github.com/mapbox/rehype-prism)
  — `refractor`を介してPrismで構文をハイライト
* [`rehype-prism-plus`](https://github.com/timlrx/rehype-prism-plus)
  — 追加機能付きの`refractor`を介してPrismで構文をハイライト
* [`rehype-raw`](https://github.com/rehypejs/rehype-raw)
  — ツリーを再度解析（および生のノード）
* [`rehype-react`](https://github.com/rehypejs/rehype-react)
  — Reactにコンパイル
* [`rehype-remark`](https://github.com/rehypejs/rehype-remark)
  — remarkサポート
* [`rehype-remove-comments`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-remove-comments)
  — コメントを削除
* [`rehype-remove-duplicate-attribute-values`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-remove-duplicate-attribute-values)
  — 重複する属性値を削除
* [`rehype-remove-empty-attribute`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-remove-empty-attribute)
  — 空の属性を削除
* [`rehype-remove-external-script-content`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-remove-external-script-content)
  — `src`を持つ`<script>`のコンテンツを削除
* [`rehype-remove-images`](https://github.com/iloveitaly/rehype-remove-images)
* [`rehype-remove-meta-http-equiv`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-remove-meta-http-equiv)
  — 特定の`http-equiv`をより短い代替案に置き換え
* [`rehype-remove-script-type-javascript`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-remove-script-type-javascript)
  — JS `<script>`の`type`と`language`を削除
* [`rehype-remove-style-type-css`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-remove-style-type-css)
  — CSS `<style>`と`<link>`の`type`を削除
* [`rehype-remove-unused-css`](https://github.com/nzt/rehype-remove-unused-css)
  — 未使用のCSSを削除
* [`rehype-resolution`](https://github.com/michaelnisi/rehype-resolution)
  — 画像に解像度`srcset`を注入
* [`rehype-retext`](https://github.com/rehypejs/rehype-retext)
  — retextサポート
* [`rehype-rewrite`](https://github.com/jaywcjlove/rehype-rewrite)
  — rehypeで要素を書き換え
* [`rehype-sanitize`](https://github.com/rehypejs/rehype-sanitize)
  — HTMLをサニタイズ
* [`rehype-scroll-to-top`](https://github.com/benjamincharity/rehype-scroll-to-top)
  — カスタマイズ可能な「トップへスクロール」と「ボトムへスクロール」リンク
* [`rehype-section`](https://github.com/agentofuser/rehype-section)
  — 見出しとその内容をネストされた`<section>`要素でラップ
* [`rehype-sectionize`](https://github.com/hbsnow/rehype-sectionize)
  — 見出しとその内容を属性付きのネストされた`<section>`要素でラップ
* [`rehype-semantic-images`](https://github.com/benjamincharity/rehype-semantic-images)
  — HTML画像をセマンティック要素とカスタマイズ可能な機能で強化
* [`rehype-shift-heading`](https://github.com/rehypejs/rehype-shift-heading)
  — 見出しのランクを変更
* [`rehype-shiki`](https://github.com/rsclarke/rehype-shiki)
  — Shikiでコードブロックの構文をハイライト
* [`rehype-slots`](https://github.com/marekweb/rehype-slots)
  — スロット要素を注入されたコンテンツで置換
* [`rehype-slug`](https://github.com/rehypejs/rehype-slug)
  — 見出しに`id`を追加
* [`rehype-slug-custom-id`](https://github.com/unicorn-utterances/rehype-slug-custom-id)
  — 見出しに`id`を追加、`{#custom-ids}`もサポート
* [`rehype-sort-attribute-values`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-sort-attribute-values)
  — 属性値をソート
* [`rehype-sort-attributes`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-sort-attributes)
  — 属性をソート
* [`rehype-sort-tailwind-classes`](https://github.com/bitcrowd/rehype-sort-tailwind-classes)
  — tailwindクラスをソート
* [`rehype-starry-night`](https://github.com/rehypejs/rehype-starry-night)
  — [`starry-night`](https://github.com/wooorm/starry-night)を使用してコードに構文ハイライトを適用
* [`rehype-svgo`](https://github.com/TomerAberbach/rehype-svgo)
  — [SVGO](https://github.com/svg/svgo)を使用してインラインSVGを最適化
* [`rehype-template`](https://github.com/nzt/rehype-template)
  — テンプレートリテラルでコンテンツをラップ
* [`rehype-toc`](https://github.com/JS-DevTools/rehype-toc)
  — ページに目次（TOC）を追加
* [`rehype-truncate`](https://github.com/luk707/rehype-truncate)
  — HTMLの構造を保持しながらHTMLを切り詰め
* [`rehype-twemojify`](https://github.com/cliid/rehype-twemojify)
  — 絵文字のショートコードをtwemojiに変換
* [`rehype-twoslash`](https://github.com/rehypejs/rehype-twoslash)
  — `twoslash`でJavaScriptとTypeScriptコードを処理し、`starry-night`でハイライト
* [`rehype-urls`](https://github.com/brechtcs/rehype-urls)
  — `href`と`src`属性のURLを書き換え
* [`rehype-url-inspector`](https://github.com/JS-DevTools/rehype-url-inspector)
  — ドキュメント内のどこでもURLを検査、検証、または書き換え
* [`rehype-video`](https://jaywcjlove.github.io/rehype-video)
  — 改善されたビデオ構文：`.mp4`と`.mov`へのリンクをビデオに変換
* [`rehype-webparser`](https://github.com/Prettyhtml/prettyhtml/tree/HEAD/packages/rehype-webparser)
  — より厳密でないHTMLパーサー
* [`rehype-widont`](https://github.com/radiojhero/rehype-widont)
  — 一語の行を防ぐ
* [`rehype-wrap`](https://github.com/mrzmmr/rehype-wrap)
  — 選択された要素を指定された要素でラップ
* [`rehype-wrap-all`](https://github.com/florentb/rehype-wrap-all)
  — マッチするすべての要素を指定された要素でラップ

## ユーティリティリスト

構文ツリーで動作するユーティリティのリストについては[hast][hast-util]を参照してください。
hastやその他の構文ツリーで動作する他のユーティリティについては[unist][unist-util]を参照してください。
最後に、仮想ファイルで動作するユーティリティのリストについては[vfile][vfile-util]を参照してください。

## プラグインの使用

プログラムでプラグインを使用するには、[`use()`][unified-use]関数を呼び出します。
`rehype-cli`でプラグインを使用するには、[`--use`フラグ][unified-args-use]を渡すか、
[設定ファイル][config-file-use]で指定します。

## プラグインの作成

プラグインを作成するには、まず[プラグインの概念][unified-plugins]について読んでください。
次に、["unifiedでプラグインを作成する"ガイド][guide]を読んでください。
最後に、作成しようとしているものに似た既存のプラグインの1つを選び、そこから作業を始めてください。
行き詰まった場合は、[discussions][]で助けを得るのが良いでしょう。

`'rehype-'`で始まる名前を選ぶべきです（例えば`rehype-format`）。
作成したものが`rehype().use()`で動作しない場合は、**`rehype-`プレフィックスを使用しないでください**：
それは"プラグイン"ではなく、ユーザーを混乱させます。
hastで動作する場合は`'hast-util-'`を、任意のunistツリーで動作する場合は
`unist-util-`を、仮想ファイルで動作する場合は`vfile-`を使用してください。

パッケージからプラグインを公開するにはデフォルトエクスポートを使用し、`package.json`に`rehype-plugin`
キーワードを追加し、GitHubのリポジトリに`rehype-plugin`トピックを追加し、
このページにプラグインを追加するプルリクエストを作成してください！

<!--定義:-->

[logo]: https://raw.githubusercontent.com/rehypejs/rehype/cb624bd/logo.svg?sanitize=true

[rehype]: https://github.com/rehypejs/rehype

[awesome-rehype]: https://github.com/rehypejs/awesome-rehype

[topic]: https://github.com/topics/rehype-plugin

[hast-util]: https://github.com/syntax-tree/hast#list-of-utilities

[unist-util]: https://github.com/syntax-tree/unist#unist-utilities

[vfile-util]: https://github.com/vfile/vfile#utilities

[unified-use]: https://github.com/unifiedjs/unified#processoruseplugin-options

[unified-args-use]: https://github.com/unifiedjs/unified-args#--use-plugin

[config-file-use]: https://github.com/unifiedjs/unified-engine/blob/main/doc/configure.md#plugins

[unified-plugins]: https://github.com/unifiedjs/unified#plugin

[guide]: https://unifiedjs.com/learn/guide/create-a-plugin/

[discussions]: https://github.com/rehypejs/rehype/discussions

[remark-plugins]: https://github.com/remarkjs/remark/blob/main/doc/plugins.md#list-of-plugins