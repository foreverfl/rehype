# rehype-parse

[![Build][build-badge]][build]
[![Coverage][coverage-badge]][coverage]
[![Downloads][downloads-badge]][downloads]
[![Size][size-badge]][size]
[![Sponsors][sponsors-badge]][collective]
[![Backers][backers-badge]][collective]
[![Chat][chat-badge]][chat]

HTMLからの解析をサポートするための**[rehype][]**プラグインです。

## 目次

* [これは何ですか？](#これは何ですか)
* [いつ使うべきですか？](#いつ使うべきですか)
* [インストール](#インストール)
* [使用方法](#使用方法)
* [API](#api)
  * [`unified().use(rehypeParse[, options])`](#unifieduserehypeparse-options)
  * [`ErrorCode`](#errorcode)
  * [`ErrorSeverity`](#errorseverity)
  * [`Options`](#options)
* [例](#例)
  * [例：フラグメントと文書の比較](#例フラグメントと文書の比較)
  * [例：`<html>`の周囲と内部の空白](#例htmlの周囲と内部の空白)
  * [例：解析エラー](#例解析エラー)
* [構文](#構文)
* [構文ツリー](#構文ツリー)
* [型](#型)
* [互換性](#互換性)
* [セキュリティ](#セキュリティ)
* [貢献](#貢献)
* [スポンサー](#スポンサー)
* [ライセンス](#ライセンス)

## これは何ですか？

このパッケージは、HTMLを入力として受け取り、構文ツリーに変換する方法を定義する[unified][] ([rehype][])プラグインです。
使用すると、HTMLを解析し、その後に他のrehypeプラグインを使用できます。

rehypeエコシステムについての情報は、[モノレポのREADME][rehype]を参照してください。

## いつ使うべきですか？

このプラグインは、HTMLを解析するためのサポートをunifiedに追加します。
HTMLをシリアライズする必要もある場合は、代わりに[`rehype`][rehype-core]を使用できます。これはunified、このプラグイン、および[`rehype-stringify`][rehype-stringify]を組み合わせたものです。

ブラウザにいて、コンテンツを信頼し、位置情報が不要で、より小さなバンドルサイズを重視する場合は、代わりに[`rehype-dom-parse`][rehype-dom-parse]を使用できます。

プラグインを使用せず、構文ツリーにアクセスしたい場合は、このプラグイン内で使用されている[`hast-util-from-html`][hast-util-from-html]を直接使用できます。
rehypeは、そのような内部を抽象化することで、コンテンツの変換をより簡単にすることに焦点を当てています。

## インストール

このパッケージは[ESMのみ][esm]です。
Node.js（バージョン16以上）では、[npm][]でインストールしてください：

```sh
npm install rehype-parse
```

Denoでは[`esm.sh`][esmsh]を使用してください：

```js
import rehypeParse from 'https://esm.sh/rehype-parse@9'
```

ブラウザでは[`esm.sh`][esmsh]を使用してください：

```html
<script type="module">
  import rehypeParse from 'https://esm.sh/rehype-parse@9?bundle'
</script>
```

## 使用方法

次のような`example.js`モジュールがあるとします：

```js
import rehypeParse from 'rehype-parse'
import rehypeRemark from 'rehype-remark'
import remarkStringify from 'remark-stringify'
import {unified} from 'unified'

const file = await unified()
  .use(rehypeParse)
  .use(rehypeRemark)
  .use(remarkStringify)
  .process('<h1>Hello, world!</h1>')

console.log(String(file))
```

...`node example.js`で実行すると、次のような結果が得られます：

```markdown
# Hello, world!
```

## API

このパッケージは識別子をエクスポートしません。
デフォルトエクスポートは[`rehypeParse`][api-rehype-parse]です。

### `unified().use(rehypeParse[, options])`

HTMLからの解析をサポートするためのプラグイン。

###### パラメータ

* `options` ([`Options`][api-options], オプション)
  — 設定

###### 戻り値

なし（`undefined`）。

### `ErrorCode`

既知の[解析エラー][parse-errors]の名前（TypeScriptタイプ）。

各エラーに関する詳細情報については、[`hast-util-from-html`][hast-util-from-html-errors]を参照してください。

###### タイプ

```ts
type ErrorCode =
  | 'abandonedHeadElementChild'
  | 'abruptClosingOfEmptyComment'
  | 'abruptDoctypePublicIdentifier'
  | 'abruptDoctypeSystemIdentifier'
  | 'absenceOfDigitsInNumericCharacterReference'
  | 'cdataInHtmlContent'
  | 'characterReferenceOutsideUnicodeRange'
  | 'closingOfElementWithOpenChildElements'
  | 'controlCharacterInInputStream'
  | 'controlCharacterReference'
  | 'disallowedContentInNoscriptInHead'
  | 'duplicateAttribute'
  | 'endTagWithAttributes'
  | 'endTagWithTrailingSolidus'
  | 'endTagWithoutMatchingOpenElement'
  | 'eofBeforeTagName'
  | 'eofInCdata'
  | 'eofInComment'
  | 'eofInDoctype'
  | 'eofInElementThatCanContainOnlyText'
  | 'eofInScriptHtmlCommentLikeText'
  | 'eofInTag'
  | 'incorrectlyClosedComment'
  | 'incorrectlyOpenedComment'
  | 'invalidCharacterSequenceAfterDoctypeName'
  | 'invalidFirstCharacterOfTagName'
  | 'misplacedDoctype'
  | 'misplacedStartTagForHeadElement'
  | 'missingAttributeValue'
  | 'missingDoctype'
  | 'missingDoctypeName'
  | 'missingDoctypePublicIdentifier'
  | 'missingDoctypeSystemIdentifier'
  | 'missingEndTagName'
  | 'missingQuoteBeforeDoctypePublicIdentifier'
  | 'missingQuoteBeforeDoctypeSystemIdentifier'
  | 'missingSemicolonAfterCharacterReference'
  | 'missingWhitespaceAfterDoctypePublicKeyword'
  | 'missingWhitespaceAfterDoctypeSystemKeyword'
  | 'missingWhitespaceBeforeDoctypeName'
  | 'missingWhitespaceBetweenAttributes'
  | 'missingWhitespaceBetweenDoctypePublicAndSystemIdentifiers'
  | 'nestedComment'
  | 'nestedNoscriptInHead'
  | 'nonConformingDoctype'
  | 'nonVoidHtmlElementStartTagWithTrailingSolidus'
  | 'noncharacterCharacterReference'
  | 'noncharacterInInputStream'
  | 'nullCharacterReference'
  | 'openElementsLeftAfterEof'
  | 'surrogateCharacterReference'
  | 'surrogateInInputStream'
  | 'unexpectedCharacterAfterDoctypeSystemIdentifier'
  | 'unexpectedCharacterInAttributeName'
  | 'unexpectedCharacterInUnquotedAttributeValue'
  | 'unexpectedEqualsSignBeforeAttributeName'
  | 'unexpectedNullCharacter'
  | 'unexpectedQuestionMarkInsteadOfTagName'
  | 'unexpectedSolidusInTag'
  | 'unknownNamedCharacterReference'
```

### `ErrorSeverity`

エラーの重大度（TypeScriptタイプ）。

* `0`または`false`
  — 解析エラーをオフにする
* `1`または`true`
  — 解析エラーを警告に変換する
* `2`
  — 解析エラーを実際のエラーに変換する：処理が停止する

###### タイプ

```ts
type ErrorSeverity = boolean | 0 | 1 | 2
```

### `Options`

設定（TypeScriptタイプ）。

> 👉 **注意**：これはXMLパーサーではありません。
> HTMLに埋め込まれたSVGをサポートしています。
> XMLで利用可能な機能はサポートしていません。
> SVGファイルを渡すと壊れる可能性がありますが、現代的なSVGのフラグメントは問題ないはずです。
> XMLを解析するには[`xast-util-from-xml`][xast-util-from-xml]を使用してください。

###### フィールド

* `fragment` (`boolean`, デフォルト: `false`)
  — フラグメントとして解析するかどうか；デフォルトでは開かれていない`html`、`head`、`body`要素が開かれます
* `emitParseErrors` (`boolean`, デフォルト: `false`)
  — 解析中に[解析エラー][parse-errors]を発生させるかどうか
* `space` (`'html'`または`'svg'`, デフォルト: `'html'`)
  — 文書がどの空間にあるか
* `verbose` (`boolean`, デフォルト: `false`)
  — 属性、開始タグ、終了タグに関する追加の位置情報を追加する
* [`[key in ErrorCode]`][api-error-code]
  ([`ErrorSeverity`][api-error-severity], デフォルト: `options.emitParseErrors`が真の場合は`1`、そうでない場合は`0`)
  — 特定の[解析エラー][parse-errors]を設定する

## 例

### 例：フラグメントと文書の比較

次の例は、文書として解析することとフラグメントとして解析することの違いを示しています：

```js
import rehypeParse from 'rehype-parse'
import rehypeStringify from 'rehype-stringify'
import {unified} from 'unified'

const doc = '<title>Hi!</title><h1>Hello!</h1>'

console.log(
  String(
    await unified()
      .use(rehypeParse, {fragment: true})
      .use(rehypeStringify)
      .process(doc)
  )
)

console.log(
  String(
    await unified()
      .use(rehypeParse, {fragment: false})
      .use(rehypeStringify)
      .process(doc)
  )
)
```

...結果：

```html
<title>Hi!</title><h1>Hello!</h1>
```

```html
<html><head><title>Hi!</title></head><body><h1>Hello!</h1></body></html>
```

> 👉 **注意**：全体の文書が期待される場合（2番目の例）、欠落している要素が開かれて閉じられます。

### 例：`<html>`の周囲と内部の空白

次の例は、`<html>`要素の周囲と直接内部の空白がどのように扱われるかを示しています：

```js
import rehypeParse from 'rehype-parse'
import rehypeStringify from 'rehype-stringify'
import {unified} from 'unified'

const doc = `<!doctype html>
<html lang=en>
  <head>
    <title>Hi!</title>
  </head>
  <body>
    <h1>Hello!</h1>
  </body>
</html>`

console.log(
  String(await unified().use(rehypeParse).use(rehypeStringify).process(doc))
)
```

...結果（`␠`は空白文字を表します）：

```html
<!doctype html><html lang="en"><head>
    <title>Hi!</title>
  </head>
  <body>
    <h1>Hello!</h1>
␠␠
</body></html>
```

> 👉 **注意**：`<html>`の前の改行は無視され、`<head>`の前の改行と2つの空白はその内部に移動され、`</body>`の後の改行はその前に移動されます。

この動作はHTML標準（セクション13.2.6.4.1「初期挿入モード」と隣接する状態を参照）に記述されており、rehypeはこれに従っています。

この意味のない空白の変更は、マークアップのフォーマット時を除いて問題にならないはずです。その場合、[`rehype-format`][rehype-format]を使用してソースコードを改善できます。

### 例：解析エラー

次の例は、HTML解析エラーを有効にし、設定する方法を示しています：

```js
import rehypeParse from 'rehype-parse'
import rehypeStringify from 'rehype-stringify'
import {unified} from 'unified'
import {reporter} from 'vfile-reporter'

const file = await unified()
  .use(rehypeParse, {
    emitParseErrors: true, // すべてを発生させる。
    missingWhitespaceBeforeDoctypeName: 2, // 1つを致命的なエラーとしてマークする。
    nonVoidHtmlElementStartTagWithTrailingSolidus: false // 1つを無視する。
  })
  .use(rehypeStringify).process(`<!doctypehtml>
<title class="a" class="b">Hello…</title>
<h1/>World!</h1>`)

console.log(reporter(file))
```

...結果：

```html
1:10-1:10 error   DOCTYPE名の前に空白がありません     missing-whitespace-before-doctype-name hast-util-from-html
2:23-2:23 warning 予期しない重複属性                   duplicate-attribute                    hast-util-from-html

2つのメッセージ (✖ 1つのエラー、⚠ 1つの警告)
```

> 🧑‍🏫 **情報**：unifiedのメッセージはエラーではなく警告です。
> 他のリンター（ESLintなど）はほとんど常にエラーを使用します。
> なぜでしょうか？
> それらのツールは*コードスタイルのみ*をチェックします。
> rehypeとunifiedが焦点を当てているコードの生成、変換、フォーマットは行いません。
> unifiedのエラーは、JavaScriptコードの例外と同じ意味を持ちます：クラッシュです。
> そのため、代わりに警告を使用します。より多くのHTMLをチェックし続け、より多くのプラグインを実行し続けるためです。

## 構文

HTMLは、WHATWG HTML（生きている標準）に従って解析されます。これはすべてのブラウザでも採用されています。

## 構文ツリー

rehypeで使用される構文ツリーのフォーマットは[hast][]です。

## 型

このパッケージは[TypeScript][]で完全に型付けされています。
追加の型[`ErrorCode`][api-error-code]、[`ErrorSeverity`][api-error-severity]、
[`Options`][api-options]をエクスポートします。

## 互換性

unified集団によって維持されているプロジェクトは、メンテナンスされているバージョンのNode.jsと互換性があります。

新しいメジャーリリースを行う際、メンテナンスされていないバージョンのNodeのサポートを削除します。
これは、現在のリリースライン`rehype-parse@^9`をNode.js 16と互換性を保つよう努めていることを意味します。

## セキュリティ

**rehype**はHTMLを扱い、HTMLの不適切な使用は[クロスサイトスクリプティング（XSS）][xss]攻撃の可能性を開く可能性があるため、rehypeの使用も安全でない可能性があります。
ツリーを安全にするには[`rehype-sanitize`][rehype-sanitize]を使用してください。

rehypeプラグインの使用も他の攻撃の可能性を開く可能性があります。
各プラグインとその使用に関連するリスクを慎重に評価してください。

レポートの提出方法については、[セキュリティポリシー][security]を参照してください。

## 貢献

[`rehypejs/.github`][health]の[`contributing.md`][contributing]を参照して、始め方を確認してください。
ヘルプを得る方法については[`support.md`][support]を参照してください。

このプロジェクトには[行動規範][coc]があります。
このリポジトリ、組織、またはコミュニティと対話することにより、その条件に従うことに同意したものとみなされます。

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

[build-badge]: https://github.com/rehypejs/rehype/workflows/main/badge.svg

[build]: https://github.com/rehypejs/rehype/actions

[coverage-badge]: https://img.shields.io/codecov/c/github/rehypejs/rehype.svg

[coverage]: https://codecov.io/github/rehypejs/rehype

[downloads-badge]: https://img.shields.io/npm/dm/rehype-parse.svg

[downloads]: https://www.npmjs.com/package/rehype-parse

[size-badge]: https://img.shields.io/bundlejs/size/rehype-parse

[size]: https://bundlejs.com/?q=rehype-parse

[sponsors-badge]: https://opencollective.com/unified/sponsors/badge.svg

[backers-badge]: https://opencollective.com/unified/backers/badge.svg

[collective]: https://opencollective.com/unified

[chat-badge]: https://img.shields.io/badge/chat-discussions-success.svg

[chat]: https://github.com/rehypejs/rehype/discussions

[security]: https://github.com/rehypejs/.github/blob/main/security.md

[health]: https://github.com/rehypejs/.github

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

[hast-util-from-html]: https://github.com/syntax-tree/hast-util-from-html

[hast-util-from-html-errors]: https://github.com/syntax-tree/hast-util-from-html#optionskey-in-errorcode

[xast-util-from-xml]: https://github.com/syntax-tree/xast-util-from-xml

[rehype-dom-parse]: https://github.com/rehypejs/rehype-dom/tree/main/packages/rehype-dom-parse

[rehype-format]: https://github.com/rehypejs/rehype-format

[rehype-sanitize]: https://github.com/rehypejs/rehype-sanitize

[parse-errors]: https://html.spec.whatwg.org/multipage/parsing.html#parse-errors

[rehype-core]: ../rehype/

[rehype-stringify]: ../rehype-stringify/

[api-error-code]: #errorcode

[api-error-severity]: #errorseverity

[api-options]: #options

[api-rehype-parse]: #unifieduserehypeparse-options