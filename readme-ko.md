# [![rehype][logo]][unified]

[![Build][build-badge]][build]
[![Coverage][coverage-badge]][coverage]
[![Downloads][downloads-badge]][downloads]
[![Size][size-badge]][size]
[![Sponsors][sponsors-badge]][collective]
[![Backers][backers-badge]][collective]
[![Chat][chat-badge]][chat]

**rehype**는 플러그인을 사용하여 HTML을 변환하는 도구입니다.
이 플러그인들은 HTML을 검사하고 변경할 수 있습니다.
rehype는 서버, 클라이언트, CLI, deno 등에서 사용할 수 있습니다.

## 소개

rehype는 HTML을 구조화된 데이터, 특히 AST(추상 구문 트리)로 다루는 플러그인 생태계입니다.
AST는 프로그램이 HTML을 쉽게 다룰 수 있게 해줍니다.
우리는 이러한 프로그램들을 플러그인이라고 부릅니다.
플러그인은 트리를 검사하고 변경합니다.
여러분은 많은 기존 플러그인을 사용하거나 직접 만들 수 있습니다.

* HTML을 배우려면 [MDN][]과 [WHATWG HTML][html]을 참조하세요
* 우리에 대해 더 알아보려면 [`unifiedjs.com`][site]을 참조하세요
* 업데이트는 [Twitter][]를 참조하세요
* 질문이 있다면 [support][]를 참조하세요
* 도움을 주고 싶다면 아래의 [contribute][] 또는 [sponsor][]를 참조하세요

## 목차

* [이것은 무엇인가요?](#이것은-무엇인가요)
* [언제 이것을 사용해야 하나요?](#언제-이것을-사용해야-하나요)
* [플러그인](#플러그인)
* [타입](#타입)
* [호환성](#호환성)
* [보안](#보안)
* [기여하기](#기여하기)
* [후원하기](#후원하기)
* [라이선스](#라이선스)

## 이것은 무엇인가요?

이 프로젝트와 플러그인을 사용하면 이런 HTML을:

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

다음과 같은 HTML로 변환할 수 있습니다:

```html
<!doctypehtml><html lang=en><meta charset=utf8><title>Saturn</title><h1>Saturn</h1><p>Saturn is a gas giant composed predominantly of hydrogen and helium.
```

<details><summary>예제 코드 보기</summary>

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

다른 플러그인을 사용하면 이런 HTML을:

```html
<h1>Hi, Saturn!</h1>
```

다음과 같은 HTML로 변환할 수 있습니다:

```html
<h2>Hi, Saturn!</h2>
```

<details><summary>예제 코드 보기</summary>

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
   * @param {Root} 트리
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

rehype는 다양한 용도로 사용할 수 있습니다.
**[unified][]** 는 AST를 사용하여 콘텐츠를 변환하는 핵심 프로젝트입니다.
**rehype**는 unified에 HTML 지원을 추가합니다.
**[hast][]** 는 rehype가 사용하는 HTML AST입니다.

이 GitHub 저장소는 다음 패키지들을 포함하는 모노레포입니다:

* [`rehype-parse`][rehype-parse]
  — HTML을 입력으로 받아 구문 트리(hast)로 변환하는 플러그인
* [`rehype-stringify`][rehype-stringify]
  — 구문 트리(hast)를 받아 HTML 출력으로 변환하는 플러그인
* [`rehype`][rehype-core]
  — `unified`, `rehype-parse`, 그리고 `rehype-stringify`를 포함하며, 입력과 출력이 모두 HTML일 때 유용함
* [`rehype-cli`][rehype-cli]
  — 스크립트에서 HTML을 검사하고 포맷팅하기 위한 `rehype` 기반의 CLI 도구

## 언제 이것을 사용해야 하나요?

가지고 있는 입력과 원하는 출력에 따라 rehype의 다른 부분을 사용할 수 있습니다.
입력이 HTML이라면 `unified`와 함께 `rehype-parse`를 사용할 수 있습니다.
출력이 HTML이라면 `unified`와 함께 `rehype-stringify`를 사용할 수 있습니다.
입력과 출력이 모두 HTML이라면 `rehype`를 단독으로 사용할 수 있습니다.
프로젝트에서 HTML 파일을 검사하고 포맷팅하고 싶다면 `rehype-cli`를 사용할 수 있습니다.

## 플러그인

rehype 플러그인은 HTML을 다룹니다.
이미 존재하는 많은 플러그인 중에서 선택할 수 있습니다.
플러그인을 찾는 좋은 방법은 세 가지가 있습니다:

* [`awesome-rehype`][awesome-rehype]
  — 가장 멋진 프로젝트들의 선택
* [플러그인 목록][list-of-plugins]
  — 모든 플러그인 목록
* [`rehype-plugin` 토픽][topic]
  — GitHub에서 태그된 모든 저장소

일부 플러그인은 여기 `@rehypejs` 조직에서 우리가 유지 관리하고 있지만, 다른 플러그인들은 다른 곳에서 유지 관리됩니다.
누구나 rehype 플러그인을 만들 수 있으므로, 항상 프로젝트에 종속성을 포함할지 결정할 때와 마찬가지로 rehype 플러그인의 품질도 신중하게 평가해야 합니다.

## 타입

rehype 조직과 unified 집단 전체는 [TypeScript][]로 완전히 타입이 지정되어 있습니다.
hast에 대한 타입은 [`@types/hast`][types-hast]에서 사용할 수 있습니다.

## 호환성

unified 집단에서 유지 관리하는 프로젝트들은 유지 관리되는 Node.js 버전과 호환됩니다.

새로운 주요 릴리스를 할 때, 우리는 유지 관리되지 않는 Node.js 버전에 대한 지원을 중단합니다.
이는 현재 릴리스 라인을 Node.js 16과 호환되도록 유지하려고 노력한다는 의미입니다.

## 보안

HTML의 부적절한 사용은 [크로스 사이트 스크립팅 (XSS)][xss] 공격에 노출될 수 있으므로, rehype의 사용도 안전하지 않을 수 있습니다.
트리를 안전하게 만들려면 [`rehype-sanitize`][rehype-sanitize]를 사용하세요.

rehype 플러그인 사용은 다른 공격에도 노출될 수 있습니다.
각 플러그인과 그 사용에 따른 위험을 신중하게 평가하세요.

보고서를 제출하는 방법에 대한 정보는 [보안 정책][security]을 참조하세요.

## 기여하기

시작하는 방법은 [`rehypejs/.github`][health]의 [`contributing.md`][contributing]를 참조하세요.
도움을 받는 방법은 [`support.md`][support]를 참조하세요.
커뮤니티 및 기여자들과 대화하려면 [Discussions][chat]에 참여하세요.

이 프로젝트에는 [행동 강령][coc]이 있습니다.
이 저장소, 조직 또는 커뮤니티와 상호 작용함으로써 여러분은 그 조건을 준수하는 데 동의하는 것입니다.

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
  <a href="https://opencollective.com/unified"><strong>여러분도 후원할 수 있습니다!</strong></a>
  <br><br>
</td>
</tr>
</table>

## 라이선스

[MIT][license] © [Titus Wormer][author]

<!-- 정의 -->

[logo]: https://raw.githubusercontent.com/rehypejs/rehype/cb624bd/logo.svg?sanitize=true

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

[license]: license

[author]: https://wooorm.com

[unified]: https://github.com/unifiedjs/unified

[types-hast]: https://github.com/DefinitelyTyped/DefinitelyTyped/tree/HEAD/types/hast

[xss]: https://en.wikipedia.org/wiki/Cross-site_scripting

[typescript]: https://www.typescriptlang.org

[mdn]: https://developer.mozilla.org/docs/Web/HTML

[html]: https://html.spec.whatwg.org/multipage/

[twitter]: https://twitter.com/unifiedjs

[site]: https://unifiedjs.com

[topic]: https://github.com/topics/rehype-plugin

[hast]: https://github.com/syntax-tree/hast

[awesome-rehype]: https://github.com/rehypejs/awesome-rehype

[rehype-sanitize]: https://github.com/rehypejs/rehype-sanitize

[rehype-parse]: packages/rehype-parse/

[rehype-stringify]: packages/rehype-stringify/

[rehype-core]: packages/rehype/

[rehype-cli]: packages/rehype-cli/

[list-of-plugins]: doc/plugins.md#list-of-plugins

[contribute]: #기여하기

[sponsor]: #후원하기