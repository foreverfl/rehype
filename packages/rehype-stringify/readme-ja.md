# rehype-stringify

[![Build][build-badge]][build]
[![Coverage][coverage-badge]][coverage]
[![Downloads][downloads-badge]][downloads]
[![Size][size-badge]][size]
[![Sponsors][sponsors-badge]][collective]
[![Backers][backers-badge]][collective]
[![Chat][chat-badge]][chat]

HTMLへのシリアライズをサポートするための**[rehype][]**プラグイン。

## 目次

* [これは何ですか？](#これは何ですか)
* [いつ使うべきですか？](#いつ使うべきですか)
* [インストール](#インストール)
* [使用方法](#使用方法)
* [API](#api)
  * [`unified().use(rehypeStringify[, options])`](#unifieduserehypestringify-options)
  * [`CharacterReferences`](#characterreferences)
  * [`Options`](#options)
* [構文](#構文)
* [構文ツリー](#構文ツリー)
* [タイプ](#タイプ)
* [互換性](#互換性)
* [セキュリティ](#セキュリティ)
* [貢献](#貢献)
* [スポンサー](#スポンサー)
* [ライセンス](#ライセンス)

## これは何ですか？

このパッケージは、構文ツリーを入力として受け取り、シリアライズされたHTMLに変換する方法を定義する[unified][] ([rehype][]) プラグインです。
使用されると、HTMLが最終結果としてシリアライズされます。

rehypeエコシステムについての情報は[モノレポのreadme][rehype]をご覧ください。

## いつ使うべきですか？

このプラグインはunifiedにHTMLのシリアライズサポートを追加します。
HTMLの解析も必要な場合は、代わりに[`rehype`][rehype-core]を使用できます。これはunified、
[`rehype-parse`][rehype-parse]、そしてこのプラグインを組み合わせたものです。

ブラウザ内で、コンテンツを信頼し、フォーマットオプションが不要で、
より小さなバンドルサイズを重視する場合は、代わりに
[`rehype-dom-stringify`][rehype-dom-stringify]を使用できます。

プラグインを使用せず、構文ツリーにアクセスできる場合は、このプラグイン内で使用されている
[`hast-util-to-html`][hast-util-to-html]を直接使用できます。
rehypeは、そのような内部実装を抽象化することで、コンテンツの変換をより簡単にすることに焦点を当てています。

別のプラグイン、[`rehype-format`][rehype-format]は、要素間に重要ではないが見栄えの良い空白を追加することで、
HTMLソースコードの読みやすさを向上させます。
また、逆の効果を望む場合、つまり最小化され難読化されたHTMLを望む場合には、
[`rehype-minify`][rehype-minify]プリセットもあります。

## インストール

このパッケージは[ESMのみ][esm]です。
Node.js（バージョン16以上）では、[npm][]でインストールしてください：

```sh
npm install rehype-stringify
```

Denoでは[`esm.sh`][esmsh]を使用してください：

```js
import rehypeStringify from 'https://esm.sh/rehype-stringify@10'
```

ブラウザでは[`esm.sh`][esmsh]を使用してください：

```html
<script type="module">
  import rehypeStringify from 'https://esm.sh/rehype-stringify@10?bundle'
</script>
```

## 使用方法

以下のような`example.js`モジュールがあるとします：

```js
import remarkRehype from 'remark-rehype'
import rehypeStringify from 'rehype-stringify'
import remarkGfm from 'remark-gfm'
import remarkParse from 'remark-parse'
import {unified} from 'unified'

const file = await unified()
  .use(remarkParse)
  .use(remarkGfm)
  .use(remarkRehype)
  .use(rehypeStringify)
  .process('# Hi\n\n*Hello*, world!')

console.log(String(file))
```

…これを`node example.js`で実行すると、以下のような結果が得られます：

```html
<h1>Hi</h1>
<p><em>Hello</em>, world!</p>
```

## API

このパッケージは識別子をエクスポートしません。
デフォルトエクスポートは[`rehypeStringify`][api-rehype-stringify]です。

### `unified().use(rehypeStringify[, options])`

HTMLへのシリアライズをサポートするためのプラグイン。

###### パラメータ

* `options` ([`Options`][api-options], オプション)
  — 設定

###### 戻り値

なし（`undefined`）。

### `CharacterReferences`

文字参照をシリアライズする方法（TypeScriptタイプ）。

> ⚠️ **注意**: `omitOptionalSemicolons`はHTMLが「解析エラー」と呼ぶものを作成しますが、
> それ以外は依然として有効なHTMLです — 最小化ツールを構築する場合を除いて使用しないでください。
> セミコロンの省略は、特定の名前付き参照や数値参照で一部のケースで可能です。

> ⚠️ **注意**: `useShortestReferences`を使用する場合、`useNamedReferences`を省略できます。

###### フィールド

* `useNamedReferences` (`boolean`, デフォルト: `false`)
  — 可能な場合は名前付き文字参照（`&amp;`）を優先します
* `omitOptionalSemicolons` (`boolean`, デフォルト: `false`)
  — 可能な場合にセミコロンを省略するかどうか
* `useShortestReferences` (`boolean`, デフォルト: `false`)
  — バイト数が少なくなる場合、可能な限り短い参照を優先します

### `Options`

設定（TypeScriptタイプ）。

> ⚠️ **危険**: コンテンツを完全に信頼している場合にのみ、`allowDangerousCharacters`と`allowDangerousHtml`を設定してください。

> 👉 **注意**: `allowParseErrors`、`bogusComments`、`tightAttributes`、および
> `tightDoctype`は意図的にマークアップ内に解析エラーを作成します（解析エラーの処理方法は
> 明確に定義されているため、これは機能しますが見た目は良くありません）。

> 👉 **注意**: これはXMLシリアライザではありません。
> HTMLに埋め込まれたSVGをサポートします。
> XMLで利用可能な機能はサポートしていません。
> XMLをシリアライズするには[`xast-util-to-xml`][xast-util-to-xml]を使用してください。

###### フィールド

* `allowDangerousCharacters` (`boolean`, デフォルト: `false`)
  — 古いブラウザでXSS脆弱性を引き起こす一部の文字をエンコードしません
* `allowDangerousHtml` (`boolean`, デフォルト: `false`)
  — [`Raw`][raw]ノードを許可し、生のHTMLとして挿入します。`false`の場合、`Raw`
  ノードはエンコードされます
* `allowParseErrors` (`boolean`, デフォルト: `false`)
  — バイトを節約するために、解析エラーを引き起こす文字をエンコードしません（動作はしますが）。
  SVG空間では使用されません。
* `bogusComments` (`boolean`, デフォルト: `false`)
  — バイトを節約するために、コメントの代わりに「偽のコメント」を使用します：`<!--charlie-->`
  の代わりに`<?charlie>`
* `characterReferences` ([`CharacterReferences`][api-character-references],
  オプション)
  — 文字参照のシリアライズ方法を設定します
* `closeEmptyElements` (`boolean`, デフォルト: `false`)
  — 内容のないSVG要素を、終了タグの代わりに開始タグにスラッシュ（`/`）をつけて閉じます：
  `<circle></circle>`の代わりに`<circle />`。
  スラッシュの前に空白を使用するかどうかを制御するには`tightSelfClosing`を参照してください。
  HTML空間では使用されません
* `closeSelfClosing` (`boolean`, デフォルト: `false`)
  — 自己閉じるノードを追加のスラッシュ（`/`）で閉じます：`<img>`の代わりに`<img />`。
  スラッシュの前に空白を使用するかどうかを制御するには`tightSelfClosing`を参照してください。
  SVG空間では使用されません。
* `collapseEmptyAttributes` (`boolean`, デフォルト: `false`)
  — 空の属性を折りたたみます：`class=""`の代わりに`class`を取得します。
  SVG空間では使用されません。
  ブール属性（`hidden`など）は常に折りたたまれます
* `omitOptionalTags` (`boolean`, デフォルト: `false`)
  — オプションの開始タグと終了タグを省略します。例えば、
  `<ol><li>one</li><li>two</li></ol>`では、両方の`</li>`終了タグを省略できます。
  最初のタグは別の`li`が続くため、最後のタグは何も続かないため省略できます。
  SVG空間では使用されません
* `preferUnquoted` (`boolean`, デフォルト: `false`)
  — バイト数が少なくなる場合、属性を引用符で囲みません。
  SVG空間では使用されません
* `quote` (`'"'`または`"'"`, デフォルト: `'"'`)
  — 使用する優先引用符
* `quoteSmart` (`boolean`, デフォルト: `false`)
  — バイト数が少なくなる場合、他の引用符を使用します
* `space` (`'html'`または`'svg'`, デフォルト: `'html'`)
  — ドキュメントが存在する空間。HTML空間で`<svg>`要素が見つかった場合、
  このパッケージは自動的にSVG空間に切り替わり、その後元に戻ります
* `tightAttributes` (`boolean`, デフォルト: `false`)
  — 可能な場合、属性を空白なしで結合してバイトを節約します：
  `class="a b" title="c d"`の代わりに`class="a b"title="c d"`を取得します。
  SVG空間では使用されません
* `tightCommaSeparatedLists` (`boolean`, デフォルト: `false`)
  — 既知のカンマ区切りの属性値を、右側にパディングを付ける代わりに
  単にカンマ（`,`）で結合します（`,␠`、ここで`␠`は空白を表します）
* `tightDoctype` (`boolean`, デフォルト: `false`)
  — バイトを節約するために、doctypeの不要な空白を削除します：
  `<!doctype html>`の代わりに`<!doctypehtml>`
* `tightSelfClosing` (`boolean`, デフォルト: `false`).
  — 自己閉じる要素を閉じる際に追加の空白を使用しません：`<img />`の代わりに`<img/>`。
  `closeSelfClosing: true`または`closeEmptyElements: true`の場合にのみ使用されます
* `upperDoctype` (`boolean`, デフォルト: `false`).
  — `<!doctype…`の代わりに`<!DOCTYPE…`を使用します。XHTML以外では無意味です
* `voids` (`Array<string>`, デフォルト:
  [`html-void-elements`][html-void-elements])
  — 終了タグなしでシリアライズする要素のタグ名。SVG空間では使用されません

## 構文

HTMLは、WHATWG HTML（リビングスタンダード）に従ってシリアライズされます。これはすべてのブラウザでも従われています。

## 構文ツリー

rehypeで使用される構文ツリーのフォーマットは[hast][]です。

## タイプ

このパッケージは[TypeScript][]で完全に型付けされています。
追加の型として[`CharacterReferences`][api-character-references]と
[`Options`][api-options]をエクスポートします。

## 互換性

unified collectiveによって維持されているプロジェクトは、メンテナンスされているNode.jsのバージョンと互換性があります。

新しいメジャーリリースを行う際、メンテナンスされていないNode.jsのバージョンのサポートを終了します。
これは、現在のリリースライン`rehype-stringify@^10`をNode.js 16と互換性を保つよう努めていることを意味します。

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
  <a href="https://opencollective.com/unified"><strong>あなたもご協力いただけますか？
</strong></a>
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

[downloads-badge]: https://img.shields.io/npm/dm/rehype-stringify.svg

[downloads]: https://www.npmjs.com/package/rehype-stringify

[size-badge]: https://img.shields.io/bundlejs/size/rehype-stringify

[size]: https://bundlejs.com/?q=rehype-stringify

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

[rehype-core]: ../rehype/

[rehype-sanitize]: https://github.com/rehypejs/rehype-sanitize

[rehype-format]: https://github.com/rehypejs/rehype-format

[rehype-minify]: https://github.com/rehypejs/rehype-minify

[rehype-dom-stringify]: https://github.com/rehypejs/rehype-dom/tree/main/packages/rehype-dom-stringify

[hast-util-to-html]: https://github.com/syntax-tree/hast-util-to-html

[xast-util-to-xml]: https://github.com/syntax-tree/xast-util-to-xml

[html-void-elements]: https://github.com/wooorm/html-void-elements

<!-- ToDo: `remark-rehype`リンクがリリースされたら使用する？ -->

[raw]: https://github.com/syntax-tree/mdast-util-to-hast?tab=readme-ov-file#raw

[api-character-references]: #characterreferences

[api-options]: #options

[api-rehype-stringify]: #unifieduserehypestringify-options