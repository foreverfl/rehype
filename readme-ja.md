# [![rehype][logo]][unified]

[![Build][build-badge]][build]
[![Coverage][coverage-badge]][coverage]
[![Downloads][downloads-badge]][downloads]
[![Size][size-badge]][size]
[![Sponsors][sponsors-badge]][collective]
[![Backers][backers-badge]][collective]
[![Chat][chat-badge]][chat]

**rehype**はプラグインを使ってHTMLを変換するツールです。
これらのプラグインはHTMLを検査し、変更することができます。
rehypeはサーバー、クライアント、CLI、denoなどで使用できます。

## はじめに

rehypeは、HTMLを構造化データ、特にAST（抽象構文木）として扱うプラグインのエコシステムです。
ASTを使用すると、プログラムがHTMLを簡単に扱えるようになります。
そのようなプログラムをプラグインと呼びます。
プラグインはツリーを検査し、変更します。
多くの既存のプラグインを使用することも、独自のプラグインを作成することもできます。

* HTMLについて学ぶには、[MDN][]と[WHATWG HTML][html]を参照してください
* 私たちについての詳細は、[`unifiedjs.com`][site]をご覧ください
* 最新情報は[Twitter][]をチェックしてください
* 質問がある場合は、[support][]をご覧ください
* 協力するには、以下の[contribute][]または[sponsor][]をご覧ください

## 目次

* [これは何ですか？](#これは何ですか)
* [いつ使うべきですか？](#いつ使うべきですか)
* [プラグイン](#プラグイン)
* [型](#型)
* [互換性](#互換性)
* [セキュリティ](#セキュリティ)
* [貢献](#貢献)
* [スポンサー](#スポンサー)
* [ライセンス](#ライセンス)

## これは何ですか？

このプロジェクトとプラグインを使用すると、次のようなHTMLを：

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Saturn</title>
  </head>
  <body>
    <h1>Saturn</h1>
    <p>Saturn is a gas giant composed predominantly of hydrogen and helium.</p>
  </body>
</html>
```

以下のようなHTMLに変換できます：

```html
<!doctypehtml><html lang=en><meta charset=utf8><title>Saturn</title><h1>Saturn</h1><p>Saturn is a gas giant composed predominantly of hydrogen and helium.
```

<details><summary>サンプルコードを表示</summary>

```js
import rehypeParse from 'rehype-parse'
import rehypePresetMinify from 'rehype-preset-minify'
import rehypeStringify from 'rehype-stringify'
import {unified} from 'unified'

const file = await unified()
  .use(rehypeParse)
  .use(rehypePresetMinify)
  .use(rehypeStringify).process(`<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Saturn</title>
  </head>
  <body>
    <h1>Saturn</h1>
    <p>Saturn is a gas giant composed predominantly of hydrogen and helium.</p>
  </body>
</html>`)

console.log(String(file))
```

</details>

別のプラグインを使用すると、次のようなHTMLを：

```html
<h1>Hi, Saturn!</h1>
```

以下のようなHTMLに変換できます：

```html
<h2>Hi, Saturn!</h2>
```

<details><summary>サンプルコードを表示</summary>

```js
/**
 * @import {Root} from 'hast'
 */

import rehypeParse from 'rehype-parse'
import rehypeStringify from 'rehype-stringify'
import {unified} from 'unified'
import {visit} from 'unist-util-visit'

const file = await unified()
  .use(rehypeParse, {fragment: true})
  .use(myRehypePluginToIncreaseHeadings)
  .use(rehypeStringify)
  .process('<h1>Hi, Saturn!</h1>')

console.log(String(file))

function myRehypePluginToIncreaseHeadings() {
  /**
   * @param {Root} ツリー
   */
  return function (tree) {
    visit(tree, 'element', function (node) {
      if (['h1', 'h2', 'h3', 'h4', 'h5'].includes(node.tagName)) {
        node.tagName = 'h' + (Number(node.tagName.charAt(1)) + 1)
      }
    })
  }
}
```

</details>

rehypeは様々な用途に使用できます。
**[unified][]**は、ASTを使用してコンテンツを変換するコアプロジェクトです。
**rehype**は、unifiedにHTML対応を追加します。
**[hast][]**は、rehypeが使用するHTML ASTです。

このGitHubリポジトリは、以下のパッケージを含むモノレポです：

* [`rehype-parse`][rehype-parse]
  — HTMLを入力として受け取り、構文ツリー（hast）に変換するプラグイン
* [`rehype-stringify`][rehype-stringify]
  — 構文ツリー（hast）を受け取り、HTMLを出力として生成するプラグイン
* [`rehype`][rehype-core]
  — `unified`、`rehype-parse`、`rehype-stringify`を含み、入力と出力がHTMLの場合に便利
* [`rehype-cli`][rehype-cli]
  — スクリプトでHTMLを検査およびフォーマットするための`rehype`を中心としたCLI

## いつ使うべきですか？

入力と出力に応じて、rehypeの異なる部分を使用できます。
入力がHTMLの場合は、`rehype-parse`を`unified`と一緒に使用できます。
出力がHTMLの場合は、`rehype-stringify`を`unified`と一緒に使用できます。
入力と出力の両方がHTMLの場合は、`rehype`を単独で使用できます。
プロジェクト内のHTMLファイルを検査およびフォーマットしたい場合は、`rehype-cli`を使用できます。

## プラグイン

rehypeプラグインはHTMLを扱います。
既に存在する多くのプラグインから選択できます。
プラグインを見つけるための優れた3つの方法があります：

* [`awesome-rehype`][awesome-rehype]
  — 最も素晴らしいプロジェクトの選択
* [プラグインのリスト][list-of-plugins]
  — すべてのプラグインのリスト
* [`rehype-plugin` トピック][topic]
  — GitHubでタグ付けされたリポジトリ

一部のプラグインは`@rehypejs`組織内で私たちによって管理されていますが、他のプラグインは他の場所で管理されています。
誰でもrehypeプラグインを作成できるため、プロジェクトに依存関係を含めるかどうかを選択する際には、常にrehypeプラグインの品質も慎重に評価してください。

## 型

rehype組織と統一された集団全体は、[TypeScript][]で完全に型付けされています。
hastの型は[`@types/hast`][types-hast]で利用可能です。

## 互換性

unified集団によって維持されているプロジェクトは、メンテナンスされているバージョンのNode.jsと互換性があります。

新しいメジャーリリースをカットする際、メンテナンスされていないバージョンのNode.jsのサポートを終了します。
これは、現在のリリースラインをNode.js 16と互換性を保つよう努めていることを意味します。

## セキュリティ

HTMLの不適切な使用は[クロスサイトスクリプティング（XSS）][xss]攻撃の可能性を開く可能性があるため、rehypeの使用も安全でない可能性があります。
ツリーを安全にするには[`rehype-sanitize`][rehype-sanitize]を使用してください。

rehypeプラグインの使用により、他の攻撃にさらされる可能性もあります。
各プラグインとその使用に伴うリスクを慎重に評価してください。

レポートの提出方法については、[セキュリティポリシー][security]をご覧ください。

## 貢献

始めるための方法については、[`rehypejs/.github`][health]の[`contributing.md`][contributing]をご覧ください。
助けを得る方法については、[`support.md`][support]をご覧ください。
コミュニティや貢献者とチャットするには、[ディスカッション][chat]にご参加ください。

このプロジェクトには[行動規範][coc]があります。
このリポジトリ、組織、またはコミュニティと対話することにより、その条項に従うことに同意したものとみなされます。

## スポンサー

この取り組みを支援し、[OpenCollective][collective]でスポンサーになることで恩返しをしてください！

<table>
<tr valign="middle">
<td width="20%" align="center" rowspan="2" colspan="2">
  <a href="https://vercel.com">Vercel</a><br><br>
  <a href="https://vercel.com"><img src="https://avatars1.githubusercontent.com/u/14985020?s=256&v=4" width="128"></a>
</td>
<td width="20%" align="center" rowspan="2" colspan="2">
  <a href="https://motif.land">Motif</a><br><br>
  <a href="https://motif.land"><img src="https://avatars1.githubusercontent.com/u/74457950?s=256&v=4" width="128"></a>
</td>
<td width="20%" align="center" rowspan="2" colspan="2">
  <a href="https://www.hashicorp.com">HashiCorp</a><br><br>
  <a href="https://www.hashicorp.com"><img src="https://avatars1.githubusercontent.com/u/761456?s=256&v=4" width="128"></a>
</td>
<td width="20%" align="center" rowspan="2" colspan="2">
  <a href="https://www.gitbook.com">GitBook</a><br><br>
  <a href="https://www.gitbook.com"><img src="https://avatars1.githubusercontent.com/u/7111340?s=256&v=4" width="128"></a>
</td>
<td width="20%" align="center" rowspan="2" colspan="2">
  <a href="https://www.gatsbyjs.org">Gatsby</a><br><br>
  <a href="https://www.gatsbyjs.org"><img src="https://avatars1.githubusercontent.com/u/12551863?s=256&v=4" width="128"></a>
</td>
</tr>
<tr valign="middle">
</tr>
<tr valign="middle">
<td width="20%" align="center" rowspan="2" colspan="2">
  <a href="https://www.netlify.com">Netlify</a><br><br>
  <!--OCはより鮮明な画像を持っています-->
  <a href="https://www.netlify.com"><img src="https://images.opencollective.com/netlify/4087de2/logo/256.png" width="128"></a>
</td>
<td width="10%" align="center">
  <a href="https://www.coinbase.com">Coinbase</a><br><br>
  <a href="https://www.coinbase.com"><img src="https://avatars1.githubusercontent.com/u/1885080?s=256&v=4" width="64"></a>
</td>
<td width="10%" align="center">
  <a href="https://themeisle.com">ThemeIsle</a><br><br>
  <a href="https://themeisle.com"><img src="https://avatars1.githubusercontent.com/u/58979018?s=128&v=4" width="64"></a>
</td>
<td width="10%" align="center">
  <a href="https://expo.io">Expo</a><br><br>
  <a href="https://expo.io"><img src="https://avatars1.githubusercontent.com/u/12504344?s=128&v=4" width="64"></a>
</td>
<td width="10%" align="center">
  <a href="https://boostnote.io">Boost Note</a><br><br>
  <a href="https://boostnote.io"><img src="https://images.opencollective.com/boosthub/6318083/logo/128.png" width="64"></a>
</td>
<td width="10%" align="center">
  <a href="https://markdown.space">Markdown Space</a><br><br>
  <a href="https://markdown.space"><img src="https://images.opencollective.com/markdown-space/e1038ed/logo/128.png" width="64"></a>
</td>
<td width="10%" align="center">
  <a href="https://www.holloway.com">Holloway</a><br><br>
  <a href="https://www.holloway.com"><img src="https://avatars1.githubusercontent.com/u/35904294?s=128&v=4" width="64"></a>
</td>
<td width="10%"></td>
<td width="10%"></td>
</tr>
<tr valign="middle">
<td width="100%" align="center" colspan="8">
  <br>
  <a href="https://opencollective.com/unified"><strong>あなたも？</strong></a>
  <br><br>
</td>
</tr>
</table>

## ライセンス

[MIT][license] © [Titus Wormer][author]

<!-- 定義 -->

[logo]: https://raw.githubusercontent.com/rehypejs/rehype/cb624bd/logo.svg?sanitize=true

[build-badge]: https://github.com/rehypejs/rehype/workflows/