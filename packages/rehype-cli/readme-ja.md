
# rehype-cli

[![Build][build-badge]][build]
[![Coverage][coverage-badge]][coverage]
[![Downloads][downloads-badge]][downloads]
[![Sponsors][sponsors-badge]][collective]
[![Backers][backers-badge]][collective]
[![Chat][chat-badge]][chat]

**[rehype][]**を使用してHTMLファイルを検査および変更するためのコマンドラインインターフェース。

## 目次

* [これは何ですか？](#これは何ですか)
* [いつ使うべきですか？](#いつ使うべきですか)
* [インストール](#インストール)
* [使用方法](#使用方法)
* [CLI](#cli)
* [互換性](#互換性)
* [セキュリティ](#セキュリティ)
* [貢献](#貢献)
* [スポンサー](#スポンサー)
* [ライセンス](#ライセンス)

## これは何ですか？

このパッケージは、ターミナルやnpmスクリプトなどでHTMLファイルを検査および変更するために使用できるコマンドラインインターフェース（CLI）です。
このCLIは、HTMLを構造化データ、特にAST（抽象構文木）として扱うプラグインのエコシステムであるrehypeを中心に構築されています。
既存のプラグインから選択するか、独自のプラグインを作成することができます。

rehypeエコシステムについての情報は[モノレポのreadme][rehype]をご覧ください。

## いつ使うべきですか？

プロジェクト内のHTMLファイルをコマンドラインから操作したい場合にこのパッケージを使用できます。
`rehype-cli`には多くのオプションがあり、多くのプラグインと組み合わせることができるので、
望むことを実行できるはずです。
そうでない場合は、スクリプトで[`rehype`][rehype-core]自体を手動で使用することもできます。

## インストール

このパッケージは[ESMのみ][esm]です。
Node.js（バージョン16以上）では、[npm][]でインストールしてください：

```sh
npm install rehype-cli
```

## 使用方法

[`rehype-format`][rehype-format]を使用して`index.html`をフォーマットする：

```sh
rehype index.html --use rehype-format --output
```

## CLI

`rehype-cli`のインターフェースは、ヘルプページ（`rehype --help`）で以下のように説明されています：

```txt
Usage: rehype [options] [path | glob ...]

  rehypeを使用してHTMLを処理するCLI

Options:

      --[no-]color                        レポートに色を指定する（デフォルトでオン）
      --[no-]config                       設定ファイルを検索する（デフォルトでオン）
  -e  --ext <extensions>                  拡張子を指定する
      --file-path <path>                  処理するパスを指定する
  -f  --frail                             警告時に1で終了する
  -h  --help                              使用法情報を出力する
      --[no-]ignore                       無視ファイルを検索する（デフォルトでオン）
  -i  --ignore-path <path>                無視ファイルを指定する
      --ignore-path-resolve-from cwd|dir  `ignore-path`のパターンをそのディレクトリまたはcwdから解決する
      --ignore-pattern <globs>            無視パターンを指定する
      --inspect                           フォーマットされた構文ツリーを出力する
  -o  --output [path]                     出力場所を指定する
  -q  --quiet                             警告とエラーのみを出力する
  -r  --rc-path <path>                    設定ファイルを指定する
      --report <reporter>                 レポーターを指定する
  -s  --setting <settings>                設定を指定する
  -S  --silent                            エラーのみを出力する
      --silently-ignore                   無視されたファイルが与えられた時に失敗しない
      --[no-]stdout                       標準出力への書き込みを指定する（デフォルトでオン）
  -t  --tree                              入力と出力を構文ツリーとして指定する
      --tree-in                           入力を構文ツリーとして指定する
      --tree-out                          構文ツリーを出力する
  -u  --use <plugins>                     プラグインを使用する
      --verbose                           メッセージの追加情報を報告する
  -v  --version                           バージョン番号を出力する
  -w  --watch                             変更を監視して再処理する

Examples:

  # `input.html`を処理する
  $ rehype input.html -o output.html

  # パイプ
  $ rehype < input.html > output.html

  # 適用可能なすべてのファイルを書き換える
  $ rehype . -o
```

これらすべてのオプションについての詳細情報は、実際の作業を行う[`unified-args`][unified-args]で利用可能です。
`rehype-cli`は以下のように事前設定された`unified-args`です：

* `rehype-`プラグインを読み込む
* HTML拡張子（`.html`、`.htm`、`.xht`、`.xhtml`）を検索する
* [`.rehypeignore`ファイル][ignore-file]で見つかったパスを無視する
* [`.rehyperc`、`.rehyperc.js`などのファイル][config-file]から設定を読み込む
* [`package.json`ファイルの`rehype`フィールド][config-file]から設定を使用する

## 互換性

unified collectiveによって維持されているプロジェクトは、メンテナンスされているNode.jsのバージョンと互換性があります。

新しいメジャーリリースを行う際、メンテナンスされていないNode.jsのバージョンのサポートを終了します。
これは、現在のリリースライン`rehype-cli@^12`をNode.js 16と互換性を保つよう努めていることを意味します。

## セキュリティ

**rehype**はHTMLを扱うため、HTMLの不適切な使用は[クロスサイトスクリプティング（XSS）][xss]攻撃に対して脆弱になる可能性があります。
したがって、rehypeの使用も安全でない可能性があります。
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

[downloads-badge]: https://img.shields.io/npm/dm/rehype-cli.svg

[downloads]: https://www.npmjs.com/package/rehype-cli

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

[rehype]: https://github.com/rehypejs/rehype

[xss]: https://en.wikipedia.org/wiki/Cross-site_scripting

[rehype-core]: ../rehype/

[rehype-format]: https://github.com/rehypejs/rehype-format

[rehype-sanitize]: https://github.com/rehypejs/rehype-sanitize

[config-file]: https://github.com/unifiedjs/unified-engine/blob/main/readme.md#config-files

[ignore-file]: https://github.com/unifiedjs/unified-engine/blob/main/readme.md#ignore-files

[unified-args]: https://github.com/unifiedjs/unified-args#cli
