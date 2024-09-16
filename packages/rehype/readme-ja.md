# rehype

[![Build][build-badge]][build]
[![Coverage][coverage-badge]][coverage]
[![Downloads][downloads-badge]][downloads]
[![Size][size-badge]][size]
[![Sponsors][sponsors-badge]][collective]
[![Backers][backers-badge]][collective]
[![Chat][chat-badge]][chat]

HTMLからのパースとHTMLへのシリアライズをサポートするための**[unified][]**プロセッサー。

## 目次

* [これは何ですか？](#これは何ですか)
* [いつ使うべきですか？](#いつ使うべきですか)
* [インストール](#インストール)
* [使用方法](#使用方法)
* [API](#api)
  * [`rehype()`](#rehype-1)
* [例](#例)
  * [例：`rehype-parse`と`rehype-stringify`にオプションを渡す](#例rehype-parseとrehype-stringifyにオプションを渡す)
* [構文](#構文)
* [構文ツリー](#構文ツリー)
* [タイプ](#タイプ)
* [互換性](#互換性)
* [セキュリティ](#セキュリティ)
* [貢献](#貢献)
* [スポンサー](#スポンサー)
* [ライセンス](#ライセンス)

## これは何ですか？

このパッケージは、[`rehype-parse`][rehype-parse]と[`rehype-stringify`][rehype-stringify]を使用して
HTMLを入力としてパースし、出力としてHTMLにシリアライズする機能をサポートする[unified][]プロセッサーです。

rehypeエコシステムについての情報は[モノレポのreadme][rehype]をご覧ください。

## いつ使うべきですか？

unifiedを使用し、HTMLを入力として持ち、HTMLを出力として望む場合にこのパッケージを使用できます。
このパッケージは`unified().use(rehypeParse).use(rehypeStringify)`の短縮形です。
入力がHTML以外の場合（つまり`rehype-parse`が不要な場合）や出力がHTML以外の場合（`rehype-stringify`が不要な場合）は、
`unified`を直接使用することをお勧めします。

ブラウザ内で使用し、コンテンツを信頼し、ノードの位置情報やフォーマットオプションが不要で、
より小さなバンドルサイズを重視する場合は、代わりに[`rehype-dom`][rehype-dom]を使用できます。

プロジェクト内のHTMLファイルを検査およびフォーマットしたい場合は、
コマンドラインで[`rehype-cli`][rehype-cli]を使用できます。

## インストール

このパッケージは[ESMのみ][esm]です。
Node.js（バージョン16以上）では、[npm][]でインストールしてください：

```sh
npm install rehype
```

Denoでは[`esm.sh`][esmsh]を使用してください：

```js
import {rehype} from 'https://esm.sh/rehype@13'
```

ブラウザでは[`esm.sh`][esmsh]を使用してください：

```html
<script type="module">
  import {rehype} from 'https://esm.sh/rehype@13?bundle'
</script>
```

## 使用方法

以下のような`example.js`モジュールがあるとします：

```js
import {rehype} from 'rehype'
import rehypeFormat from 'rehype-format'

const file = await rehype().use(rehypeFormat).process(`<!doctype html>
        <html lang=en>
<head>
    <title>Hi!</title>
  </head>
  <body>
    <h1>Hello!</h1>

</body></html>`)

console.error(String(file))
```

…`node example.js`で実行すると、次のような結果が得られます：

```html
<!doctype html>
<html lang="en">
  <head>
    <title>Hi!</title>
  </head>
  <body>
    <h1>Hello!</h1>
  </body>
</html>
```

## API

このパッケージは[`rehype`][api-rehype]識別子をエクスポートします。
デフォルトエクスポートはありません。

### `rehype()`

[`rehype-parse`][rehype-parse]と[`rehype-stringify`][rehype-stringify]をすでに使用している
新しいunifiedプロセッサーを作成します。

`use`を使用してさらにプラグインを追加できます。
詳細については[`unified`][unified]を参照してください。

## 例

### 例：`rehype-parse`と`rehype-stringify`にオプションを渡す

`rehype-parse`または`rehype-stringify`を手動で使用する場合、`use`で直接オプションを渡すことができます。
しかし、両方のプラグインがすでに`rehype`で使用されているため、それは不可能です。
代わりに、`data`にオプションを渡すことでそれらのオプションを定義できます：

```js
import {rehype} from 'rehype'
import {reporter} from 'vfile-reporter'

const file = await rehype()
  .data('settings', {
    emitParseErrors: true,
    fragment: true,
    preferUnquoted: true
  })
  .process('<div title="a" title="b"></div>')

console.error(reporter(file))
console.log(String(file))
```

…結果：

```txt
1:21-1:21 warning Unexpected duplicate attribute duplicate-attribute hast-util-from-html

⚠ 1 warning
```

```html
<div title=a></div>
```

## 構文

HTMLは、WHATWG HTML（現在の標準）に従ってパースおよびシリアライズされます。
これはすべてのブラウザでも従われています。

## 構文ツリー

rehypeで使用される構文ツリーの形式は[hast][]です。

## タイプ

このパッケージは[TypeScript][]で完全に型付けされています。
追加の型はエクスポートしていません。

## 互換性

unified collectiveによって維持されているプロジェクトは、メンテナンスされているNode.jsのバージョンと互換性があります。

新しいメジャーリリースを行う際、メンテナンスされていないNode.jsのバージョンのサポートを終了します。
これは、現在のリリースライン`rehype@^13`をNode.js 16と互換性を保つよう努めていることを意味します。

## セキュリティ

**rehype**はHTMLを扱うため、HTMLの不適切な使用は
[クロスサイトスクリプティング（XSS）][xss]攻撃に対して脆弱になる可能性があります。
ツリーを安全にするには[`rehype-sanitize`][rehype-sanitize]を使用してください。

rehypeプラグインの使用は、他の攻撃にもさらされる可能性があります。
各プラグインとその使用に伴うリスクを慎重に評価してください。

レポートの提出方法については、[セキュリティポリシー][security]をご覧ください。

## 貢献

始め方については、[`rehypejs/.github`][health]の[`contributing.md`][contributing]をご覧ください。
ヘルプを得る方法については[`support.md`][support]をご覧ください。

このプロジェクトには[行動規範][coc]があります。
このリポジトリ、組織、またはコミュニティとやり取りすることで、あなたはその条件に従うことに同意したことになります。

## スポンサー

この取り組みを支援し、[OpenCollective][collective]でスポンサーになることで還元してください！

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
  <!--OC has a sharper image-->
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


[build-badge]: https://github.com/rehypejs/rehype/workflows/main/badge.svg

[build]: https://github.com/rehypejs/rehype/actions

[coverage-badge]: https://img.shields.io/codecov/c/github/rehypejs/rehype.svg

[coverage]: https://codecov.io/github/rehypejs/rehype

[downloads-badge]: https://img.shields.io/npm/dm/rehype.svg

[downloads]: https://www.npmjs.com/package/rehype

[size-badge]: https://img.shields.io/bundlejs/size/rehype

[size]: https://bundlejs.com/?q=rehype

[sponsors-badge]: https://opencollective.com/unified/sponsors/badge.svg

[backers-badge]: https://opencollective.com/unified/backers/badge.svg

[collective]: https://opencollective.com/unified

[chat-badge]: https://img.shields.io/badge/chat-discussions-success.svg

[chat]: https://github.com/rehypejs/rehype/discussions

[health]: https://github.com/rehypejs/.github

[security]: https://github.com/rehypejs/.github/blob/main/security.md

[contributing]: https://github.com/rehypejs/.github/blob/main/contributing.md

[support]: https://github.com/rehypejs/.github/blob/main/support.md

[coc]: https://github.com/rehypejs/.github/blob/main/code-of-conduct.md

[license]: https://github.com/rehypejs/rehype/blob/main/license

[author]: https://wooorm.com

[esm]: https://gist.github.com/sindresorhus/a39789f98801d908bbc7ff3ecc99d99c

[npm]: https://docs.npmjs.com/cli/install

[esmsh]: https://esm.sh

[unified]: https://github.com/unifiedjs/unified

[rehype]: https://github.com/rehypejs/rehype

[hast]: https://github.com/syntax-tree/hast

[xss]: https://en.wikipedia.org/wiki/Cross-site_scripting

[typescript]: https://www.typescriptlang.org

[rehype-parse]: ../rehype-parse/

[rehype-stringify]: ../rehype-stringify/

[rehype-cli]: ../rehype-cli/

[rehype-sanitize]: https://github.com/rehypejs/rehype-sanitize

[rehype-dom]: https://github.com/rehypejs/rehype-dom/tree/main/packages/rehype-dom

[api-rehype]: #rehype-1