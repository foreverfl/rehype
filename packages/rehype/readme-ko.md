# rehype

[![Build][build-badge]][build]
[![Coverage][coverage-badge]][coverage]
[![Downloads][downloads-badge]][downloads]
[![Size][size-badge]][size]
[![Sponsors][sponsors-badge]][collective]
[![Backers][backers-badge]][collective]
[![Chat][chat-badge]][chat]

HTML을 파싱하고 HTML로 직렬화하는 지원을 추가하기 위한 **[unified][]** 프로세서입니다.

## 목차

* [이게 무엇인가요?](#이게-무엇인가요)
* [언제 사용해야 하나요?](#언제-사용해야-하나요)
* [설치](#설치)
* [사용법](#사용법)
* [API](#api)
  * [`rehype()`](#rehype-1)
* [예시](#예시)
  * [예시: `rehype-parse`와 `rehype-stringify`에 옵션 전달하기](#예시-rehype-parse와-rehype-stringify에-옵션-전달하기)
* [구문](#구문)
* [구문 트리](#구문-트리)
* [타입](#타입)
* [호환성](#호환성)
* [보안](#보안)
* [기여하기](#기여하기)
* [후원하기](#후원하기)
* [라이선스](#라이선스)

## 이게 무엇인가요?

이 패키지는 [`rehype-parse`][rehype-parse]와 [`rehype-stringify`][rehype-stringify]를 사용하여 
HTML을 입력으로 파싱하고 출력으로 HTML을 직렬화하는 기능을 지원하는 [unified][] 프로세서입니다.

rehype 생태계에 대한 정보는 [모노레포의 readme][rehype]를 참조하세요.

## 언제 사용해야 하나요?

unified를 사용하고, HTML을 입력으로 받아 HTML을 출력으로 원할 때 이 패키지를 사용할 수 있습니다.
이 패키지는 `unified().use(rehypeParse).use(rehypeStringify)`의 단축형입니다.
입력이 HTML이 아니거나(즉, `rehype-parse`가 필요 없는 경우) 출력이 HTML이 아닌 경우(즉, `rehype-stringify`가 필요 없는 경우)에는 
`unified`를 직접 사용하는 것이 좋습니다.

브라우저에서 사용하고, 콘텐츠를 신뢰하며, 노드의 위치 정보나 포맷팅 옵션이 필요 없고, 더 작은 번들 크기를 원한다면
[`rehype-dom`][rehype-dom]을 대신 사용할 수 있습니다.

프로젝트에서 HTML 파일을 검사하고 포맷팅하려면 커맨드 라인에서 [`rehype-cli`][rehype-cli]를 사용할 수 있습니다.

## 설치

이 패키지는 [ESM 전용][esm]입니다.
Node.js(버전 16+)에서는 [npm][]으로 설치하세요:

```sh
npm install rehype
```

Deno에서는 [`esm.sh`][esmsh]를 사용하세요:

```js
import {rehype} from 'https://esm.sh/rehype@13'
```

브라우저에서는 [`esm.sh`][esmsh]를 사용하세요:

```html
<script type="module">
  import {rehype} from 'https://esm.sh/rehype@13?bundle'
</script>
```

## 사용법

다음과 같은 `example.js` 모듈이 있다고 가정해봅시다:

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

...`node example.js`로 실행하면 다음과 같은 결과가 나옵니다:

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

이 패키지는 [`rehype`][api-rehype] 식별자를 내보냅니다.
기본 내보내기는 없습니다.

### `rehype()`

이미 [`rehype-parse`][rehype-parse]와 [`rehype-stringify`][rehype-stringify]를 사용하는 
새로운 unified 프로세서를 생성합니다.

`use`를 사용하여 더 많은 플러그인을 추가할 수 있습니다.
자세한 내용은 [`unified`][unified]를 참조하세요.

## 예시

### 예시: `rehype-parse`와 `rehype-stringify`에 옵션 전달하기

`rehype-parse` 또는 `rehype-stringify`를 수동으로 사용할 때는 `use`로 직접 옵션을 전달할 수 있습니다.
하지만 두 플러그인이 이미 `rehype`에서 사용되고 있기 때문에 그렇게 할 수 없습니다.
대신 `data`에 옵션을 전달하여 정의할 수 있습니다:

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

...결과:

```txt
1:21-1:21 warning Unexpected duplicate attribute duplicate-attribute hast-util-from-html

⚠ 1 warning
```

```html
<div title=a></div>
```

## 구문

HTML은 WHATWG HTML(현재 표준)에 따라 파싱되고 직렬화되며, 이는 모든 브라우저에서도 따르고 있습니다.

## 구문 트리

rehype에서 사용되는 구문 트리 형식은 [hast][]입니다.

## 타입

이 패키지는 [TypeScript][]로 완전히 타입이 지정되어 있습니다.
추가적인 타입을 내보내지 않습니다.

## 호환성

unified 그룹에서 유지 관리하는 프로젝트들은 Node.js의 유지 관리 버전과 호환됩니다.

새로운 메이저 릴리스를 할 때, 우리는 Node.js의 유지 관리되지 않는 버전에 대한 지원을 중단합니다.
이는 현재 릴리스 라인인 `rehype@^13`을 Node.js 16과 호환되도록 유지하려 노력한다는 의미입니다.

## 보안

**rehype**는 HTML을 다루기 때문에, rehype의 부적절한 사용은 
[크로스 사이트 스크립팅(XSS)][xss] 공격에 취약할 수 있습니다.
트리를 안전하게 만들려면 [`rehype-sanitize`][rehype-sanitize]를 사용하세요.

rehype 플러그인의 사용은 다른 공격에도 노출될 수 있습니다.
각 플러그인과 그 사용에 따른 위험을 신중히 평가하세요.

보고서를 제출하는 방법에 대한 정보는 [보안 정책][security]을 참조하세요.

## 기여하기

시작하는 방법은 [`rehypejs/.github`][health]의 [`contributing.md`][contributing]를 참조하세요.
도움을 받는 방법은 [`support.md`][support]를 참조하세요.

이 프로젝트에는 [행동 강령][coc]이 있습니다.
이 저장소, 조직 또는 커뮤니티와 상호 작용함으로써 여러분은 이 조건을 준수하는 데 동의하는 것입니다.

## 후원하기

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

[unified]: https://github.com/foreverfl/unified/blob/main/readme-ko.md

[rehype]: https://github.com/rehypejs/rehype

[hast]: https://github.com/syntax-tree/hast

[xss]: https://en.wikipedia.org/wiki/Cross-site_scripting

[typescript]: https://www.typescriptlang.org

[rehype-parse]: ../rehype-parse/readme-ko.md

[rehype-stringify]: ../rehype-stringify/readme-ko.md

[rehype-cli]: ../rehype-cli/readme-ko.md

[rehype-sanitize]: https://github.com/rehypejs/rehype-sanitize

[rehype-dom]: https://github.com/rehypejs/rehype-dom/tree/main/packages/rehype-dom

[api-rehype]: #rehype-1