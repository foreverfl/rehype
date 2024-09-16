# rehype-parse

[![Build][build-badge]][build]
[![Coverage][coverage-badge]][coverage]
[![Downloads][downloads-badge]][downloads]
[![Size][size-badge]][size]
[![Sponsors][sponsors-badge]][collective]
[![Backers][backers-badge]][collective]
[![Chat][chat-badge]][chat]

HTML 파싱을 지원하는 **[rehype][]** 플러그인입니다.

## 목차

* [이것은 무엇인가요?](#이것은-무엇인가요)
* [언제 사용해야 하나요?](#언제-사용해야-하나요)
* [설치](#설치)
* [사용법](#사용법)
* [API](#api)
  * [`unified().use(rehypeParse[, options])`](#unifieduserehypeparse-options)
  * [`ErrorCode`](#errorcode)
  * [`ErrorSeverity`](#errorseverity)
  * [`Options`](#options)
* [예제](#예제)
  * [예제: 프래그먼트 vs 문서](#예제-프래그먼트-vs-문서)
  * [예제: `<html>` 주변과 내부의 공백](#예제-html-주변과-내부의-공백)
  * [예제: 파싱 오류](#예제-파싱-오류)
* [구문](#구문)
* [구문 트리](#구문-트리)
* [타입](#타입)
* [호환성](#호환성)
* [보안](#보안)
* [기여](#기여)
* [후원](#후원)
* [라이선스](#라이선스)

## 이것은 무엇인가요?

이 패키지는 HTML을 입력으로 받아 구문 트리로 변환하는 방법을 정의하는 [unified][] ([rehype][]) 플러그인입니다.
이를 사용하면 HTML을 파싱할 수 있고 다른 rehype 플러그인을 그 후에 사용할 수 있습니다.

rehype 생태계에 대한 정보는 [monorepo readme][rehype]를 참조하세요.

## 언제 사용해야 하나요?

이 플러그인은 unified에 HTML 파싱 지원을 추가합니다.
HTML을 직렬화해야 하는 경우, 대신 unified, 이 플러그인, 그리고 [`rehype-stringify`][rehype-stringify]를 결합한 [`rehype`][rehype-core]를 사용할 수 있습니다.

브라우저에 있고, 콘텐츠를 신뢰하며, 위치 정보가 필요하지 않고, 더 작은 번들 크기를 원한다면 대신 [`rehype-dom-parse`][rehype-dom-parse]를 사용할 수 있습니다.

플러그인을 사용하지 않고 구문 트리에 접근하려면 이 플러그인 내부에서 사용되는 [`hast-util-from-html`][hast-util-from-html]을 직접 사용할 수 있습니다.
rehype는 이러한 내부를 추상화하여 콘텐츠 변환을 더 쉽게 만드는 데 중점을 둡니다.

## 설치

이 패키지는 [ESM 전용][esm]입니다.
Node.js (버전 16+)에서는 [npm][]으로 설치하세요:

```sh
npm install rehype-parse
```

Deno에서는 [`esm.sh`][esmsh]를 사용하세요:

```js
import rehypeParse from 'https://esm.sh/rehype-parse@9'
```

브라우저에서는 [`esm.sh`][esmsh]를 사용하세요:

```html
<script type="module">
  import rehypeParse from 'https://esm.sh/rehype-parse@9?bundle'
</script>
```

## 사용법

다음과 같은 `example.js` 모듈이 있다고 가정해 봅시다:

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

...`node example.js`로 실행하면 다음과 같은 결과가 나옵니다:

```markdown
# Hello, world!
```

## API

이 패키지는 식별자를 내보내지 않습니다.
기본 내보내기는 [`rehypeParse`][api-rehype-parse]입니다.

### `unified().use(rehypeParse[, options])`

HTML 파싱 지원을 추가하는 플러그인입니다.

###### 매개변수

* `options` ([`Options`][api-options], 선택사항)
  — 구성

###### 반환값

없음 (`undefined`).

### `ErrorCode`

알려진 [파싱 오류][parse-errors]의 이름 (TypeScript 타입).

각 오류에 대한 자세한 정보는 [`hast-util-from-html`][hast-util-from-html-errors]를 참조하세요.

###### 타입

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

오류 심각도 (TypeScript 타입).

* `0` 또는 `false`
  — 파싱 오류를 끕니다
* `1` 또는 `true`
  — 파싱 오류를 경고로 변환합니다
* `2`
  — 파싱 오류를 실제 오류로 변환합니다: 처리가 중지됩니다

###### 타입

```ts
type ErrorSeverity = boolean | 0 | 1 | 2
```

### `Options`

구성 (TypeScript 타입).

> 👉 **참고**: 이것은 XML 파서가 아닙니다.
> HTML에 포함된 SVG를 지원합니다.
> XML에서 사용할 수 있는 기능은 지원하지 않습니다.
> SVG 파일을 전달하면 문제가 발생할 수 있지만 현대적인 SVG 조각은 괜찮아야 합니다.
> XML을 파싱하려면 [`xast-util-from-xml`][xast-util-from-xml]을 사용하세요.

###### 필드

* `fragment` (`boolean`, 기본값: `false`)
  — 프래그먼트로 파싱할지 여부; 기본적으로 열리지 않은 `html`, `head`, `body` 요소가 열립니다
* `emitParseErrors` (`boolean`, 기본값: `false`)
  — 파싱 중 [파싱 오류][parse-errors]를 발생시킬지 여부
* `space` (`'html'` 또는 `'svg'`, 기본값: `'html'`)
  — 문서가 어떤 공간에 있는지
* `verbose` (`boolean`, 기본값: `false`)
  — 속성, 시작 태그, 끝 태그에 대한 추가 위치 정보 추가
* [`[key in ErrorCode]`][api-error-code]
  ([`ErrorSeverity`][api-error-severity], 기본값: `options.emitParseErrors`가 true이면 `1`, 그렇지 않으면 `0`)
  — 특정 [파싱 오류][parse-errors] 구성

## 예제

### 예제: 프래그먼트 vs 문서

다음 예제는 문서로 파싱하는 것과 프래그먼트로 파싱하는 것의 차이를 보여줍니다:

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

...결과:

```html
<title>Hi!</title><h1>Hello!</h1>
```

```html
<html><head><title>Hi!</title></head><body><h1>Hello!</h1></body></html>
```

> 👉 **참고**: 전체 문서가 예상되는 경우(두 번째 예제) 누락된 요소가 열리고 닫힙니다.

### 예제: `<html>` 주변과 내부의 공백

다음 예제는 `<html>` 요소 주변과 직접 내부의 공백이 어떻게 처리되는지 보여줍니다:

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

...결과 (`␠`는 공백 문자를 나타냅니다):

```html
<!doctype html><html lang="en"><head>
    <title>Hi!</title>
  </head>
  <body>
    <h1>Hello!</h1>
␠␠
</body></html>
```

> 👉 **참고**: `<html>` 앞의 줄 끝이 무시되고, `<head>` 앞의 줄 끝과 두 개의 공백이 그 안으로 이동되며, `</body>` 뒤의 줄 끝이 그 앞으로 이동됩니다.

이 동작은 rehype가 따르는 HTML 표준에 설명되어 있습니다 (13.2.6.4.1 "초기 삽입 모드" 섹션 및 인접 상태 참조).

이 무의미한 공백의 변경은 중요하지 않아야 하지만, 마크업 서식을 지정할 때는 [`rehype-format`][rehype-format]을 사용하여 소스 코드를 개선할 수 있습니다.

### 예제: 파싱 오류

다음 예제는 HTML 파싱 오류를 활성화하고 구성하는 방법을 보여줍니다:

```js
import rehypeParse from 'rehype-parse'
import rehypeStringify from 'rehype-stringify'
import {unified} from 'unified'
import {reporter} from 'vfile-reporter'

const file = await unified()
  .use(rehypeParse, {
    emitParseErrors: true, // 모든 오류를 발생시킵니다.
    missingWhitespaceBeforeDoctypeName: 2, // 하나를 치명적인 오류로 표시합니다.
    nonVoidHtmlElementStartTagWithTrailingSolidus: false // 하나를 무시합니다.
  })
  .use(rehypeStringify).process(`<!doctypehtml>
<title class="a" class="b">Hello…</title>
<h1/>World!</h1>`)

console.log(reporter(file))
```

...결과:

```html
1:10-1:10 error   Missing whitespace before doctype name missing-whitespace-before-doctype-name hast-util-from-html
2:23-2:23 warning Unexpected duplicate attribute         duplicate-attribute                    hast-util-from-html

2 messages (✖ 1 error, ⚠ 1 warning)
```

> 🧑‍🏫 **정보**: unified의 메시지는 오류 대신 경고입니다.
> 다른 린터(예: ESLint)는 거의 항상 오류를 사용합니다.
> 왜일까요?
> 그러한 도구들은 *오직* 코드 스타일만 검사합니다.
> 그들은 rehype와 unified가 초점을 맞추는 코드 생성, 변환, 포맷팅을 하지 않습니다.
> unified의 오류는 JavaScript 코드의 예외와 같습니다: 크래시를 의미합니다.
> 그래서 우리는 대신 경고를 사용합니다. 왜냐하면 우리는 더 많은 HTML을 계속 검사하고 더 많은 플러그인을 계속 실행하기 때문입니다.

## 구문

HTML은 WHATWG HTML(살아있는 표준)에 따라 파싱되며, 이는 모든 브라우저에서도 따르고 있습니다.

## 구문 트리

rehype에서 사용되는 구문 트리 형식은 [hast][]입니다.

## 타입

이 패키지는 [TypeScript][]로 완전히 타입이 지정되어 있습니다.
추가로 [`ErrorCode`][api-error-code], [`ErrorSeverity`][api-error-severity], [`Options`][api-options] 타입을 내보냅니다.

## 호환성

unified 그룹에서 유지 관리하는 프로젝트는 유지 관리되는 Node.js 버전과 호환됩니다.

새로운 메이저 릴리스를 할 때, 우리는 유지 관리되지 않는 Node.js 버전에 대한 지원을 중단합니다.
이는 현재 릴리스 라인인 `rehype-parse@^9`를 Node.js 16과 호환되도록 유지하려 노력한다는 것을 의미합니다.

## 보안

**rehype**는 HTML로 작업하며 HTML을 부적절하게 사용하면 [크로스 사이트 스크립팅 (XSS)][xss] 공격에 노출될 수 있으므로, rehype의 사용도 안전하지 않을 수 있습니다.
트리를 안전하게 만들려면 [`rehype-sanitize`][rehype-sanitize]를 사용하세요.

rehype 플러그인을 사용하면 다른 공격에도 노출될 수 있습니다.
각 플러그인과 그 사용에 따른 위험을 신중히 평가하세요.

보고서를 제출하는 방법에 대한 정보는 [보안 정책][security]을 참조하세요.

## 기여

[`contributing.md`][contributing]에서 [`rehypejs/.github`][health]를 참조하여 시작하는 방법을 확인하세요.
도움을 받는 방법은 [`support.md`][support]를 참조하세요.

이 프로젝트에는 [행동 강령][coc]이 있습니다.
이 저장소, 조직 또는 커뮤니티와 상호 작용함으로써 귀하는 이 조건을 준수하는 데 동의합니다.

## 후원

[OpenCollective][collective]에서 후원하여 이 노력을 지원하고 보답하세요!

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
  <a href="https://opencollective.com/unified"><strong>당신도?</strong></a>
  <br><br>
</td>
</tr>
</table>

## 라이선스

[MIT][license] © [Titus Wormer][author]

<!-- 정의 -->

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

[rehype]: https://github.com/foreverfl/rehype/blob/main/readme-ko.md

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