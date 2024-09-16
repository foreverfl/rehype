# rehype-stringify

[![Build][build-badge]][build]
[![Coverage][coverage-badge]][coverage]
[![Downloads][downloads-badge]][downloads]
[![Size][size-badge]][size]
[![Sponsors][sponsors-badge]][collective]
[![Backers][backers-badge]][collective]
[![Chat][chat-badge]][chat]

HTML로 직렬화하는 기능을 추가하는 **[rehype][]** 플러그인입니다.

## 목차

* [이게 뭔가요?](#이게-뭔가요)
* [언제 사용해야 하나요?](#언제-사용해야-하나요)
* [설치](#설치)
* [사용법](#사용법)
* [API](#api)
  * [`unified().use(rehypeStringify[, options])`](#unifieduserehypestringify-options)
  * [`CharacterReferences`](#characterreferences)
  * [`Options`](#options)
* [구문](#구문)
* [구문 트리](#구문-트리)
* [타입](#타입)
* [호환성](#호환성)
* [보안](#보안)
* [기여하기](#기여하기)
* [후원하기](#후원하기)
* [라이선스](#라이선스)

## 이게 뭔가요?

이 패키지는 구문 트리를 입력으로 받아 직렬화된 HTML로 변환하는 방법을 정의하는 [unified][] ([rehype][]) 플러그인입니다.
사용되면 HTML이 최종 결과물로 직렬화됩니다.

rehype 생태계에 대한 정보는 [rehype 모노레포 readme][rehype]를 참조하세요.

## 언제 사용해야 하나요?

이 플러그인은 unified에 HTML 직렬화 지원을 추가합니다.
HTML 파싱도 필요하다면 [`rehype`][rehype-core]를 대신 사용할 수 있습니다. 이는 unified,
[`rehype-parse`][rehype-parse], 그리고 이 플러그인을 결합한 것입니다.

브라우저에서 사용하고, 콘텐츠를 신뢰하며, 포맷팅 옵션이 필요 없고, 더 작은 번들 크기를 원한다면
[`rehype-dom-stringify`][rehype-dom-stringify]를 대신 사용할 수 있습니다.

플러그인을 사용하지 않고 구문 트리에 접근할 수 있다면, 이 플러그인 내부에서 사용되는
[`hast-util-to-html`][hast-util-to-html]을 직접 사용할 수 있습니다.
rehype는 이러한 내부 구현을 추상화하여 콘텐츠 변환을 더 쉽게 만드는 데 중점을 둡니다.

다른 플러그인인 [`rehype-format`][rehype-format]은 요소 사이에 중요하지 않지만 보기 좋은 공백을 추가하여
HTML 소스 코드의 가독성을 향상시킵니다.
또한 [`rehype-minify`][rehype-minify] 프리셋은 반대로 최소화되고 변형된 HTML을 원할 때 사용할 수 있습니다.

## 설치

이 패키지는 [ESM 전용][esm]입니다.
Node.js (버전 16+)에서는 [npm][]으로 설치하세요:

```sh
npm install rehype-stringify
```

Deno에서는 [`esm.sh`][esmsh]를 사용하세요:

```js
import rehypeStringify from 'https://esm.sh/rehype-stringify@10'
```

브라우저에서는 [`esm.sh`][esmsh]를 사용하세요:

```html
<script type="module">
  import rehypeStringify from 'https://esm.sh/rehype-stringify@10?bundle'
</script>
```

## 사용법

다음과 같은 `example.js` 모듈이 있다고 가정해봅시다:

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

`node example.js`로 실행하면 다음과 같은 결과가 출력됩니다:

```html
<h1>Hi</h1>
<p><em>Hello</em>, world!</p>
```

## API

이 패키지는 식별자(identifier)를 내보내지 않습니다.
기본 내보내기는 [`rehypeStringify`][api-rehype-stringify]입니다.

### `unified().use(rehypeStringify[, options])`

HTML로 직렬화하는 기능을 추가하는 플러그인입니다.

###### 매개변수

* `options` ([`Options`][api-options], 선택적)
  — 설정

###### 반환값

없음 (`undefined`).

### `CharacterReferences`

문자 참조를 직렬화하는 방법 (TypeScript 타입).

> ⚠️ **참고**: `omitOptionalSemicolons`는 HTML에서 "파싱 오류"라고 부르는 것을 생성하지만
> 그 외에는 여전히 유효한 HTML입니다 — 최소화 도구를 만들 때 외에는 사용하지 마세요.
> 세미콜론 생략은 특정 명명된 참조와 숫자 참조에서 일부 경우에 가능합니다.

> ⚠️ **참고**: `useShortestReferences`를 사용할 때는 `useNamedReferences`를 생략할 수 있습니다.

###### 필드

* `useNamedReferences` (`boolean`, 기본값: `false`)
  — 가능한 경우 명명된 문자 참조(`&amp;`)를 선호합니다
* `omitOptionalSemicolons` (`boolean`, 기본값: `false`)
  — 가능한 경우 세미콜론을 생략할지 여부
* `useShortestReferences` (`boolean`, 기본값: `false`)
  — 더 적은 바이트로 표현될 경우 가장 짧은 가능한 참조를 선호합니다

### `Options`

설정 (TypeScript 타입).

> ⚠️ **위험**: 콘텐츠를 완전히 신뢰하는 경우에만 `allowDangerousCharacters`와 `allowDangerousHtml`을 설정하세요.

> 👉 **참고**: `allowParseErrors`, `bogusComments`, `tightAttributes`, 그리고 `tightDoctype`는
> 의도적으로 마크업에서 파싱 오류를 생성합니다 (파싱 오류 처리 방법은 잘 정의되어 있어 작동하지만 좋아 보이지는 않습니다).

> 👉 **참고**: 이것은 XML 직렬화기가 아닙니다.
> HTML에 내장된 SVG를 지원합니다.
> XML에서 사용 가능한 기능은 지원하지 않습니다.
> XML을 직렬화하려면 [`xast-util-to-xml`][xast-util-to-xml]을 사용하세요.

###### 필드

* `allowDangerousCharacters` (`boolean`, 기본값: `false`)
  — 오래된 브라우저에서 XSS 취약점을 일으키는 일부 문자를 인코딩하지 않습니다
* `allowDangerousHtml` (`boolean`, 기본값: `false`)
  — [`Raw`][raw] 노드를 허용하고 원시 HTML로 삽입합니다; `false`일 경우, `Raw` 노드는 인코딩됩니다
* `allowParseErrors` (`boolean`, 기본값: `false`)
  — 바이트를 절약하기 위해 파싱 오류를 일으키는 문자를 인코딩하지 않습니다 (작동은 하지만); SVG 공간에서는 사용되지 않습니다.
* `bogusComments` (`boolean`, 기본값: `false`)
  — 바이트를 절약하기 위해 주석 대신 "bogus 주석"을 사용합니다: `<!--charlie-->`대신 `<?charlie>` 
* `characterReferences` ([`CharacterReferences`][api-character-references],
  선택적)
  — 문자 참조를 직렬화하는 방법을 설정합니다
* `closeEmptyElements` (`boolean`, 기본값: `false`)
  — 내용이 없는 SVG 요소를 닫을 때 끝 태그 대신 여는 태그에 슬래시(`/`)를 사용합니다: `<circle></circle>` 대신 `<circle />`; 
  슬래시 앞에 공백을 사용할지 여부를 제어하려면 `tightSelfClosing`을 참조하세요.
  HTML 공간에서는 사용되지 않습니다
* `closeSelfClosing` (`boolean`, 기본값: `false`)
  — 자체 닫힘 노드를 추가 슬래시(`/`)로 닫습니다: `<img>` 대신 `<img />`; 
  슬래시 앞에 공백을 사용할지 여부를 제어하려면 `tightSelfClosing`을 참조하세요. 
  SVG 공간에서는 사용되지 않습니다.
* `collapseEmptyAttributes` (`boolean`, 기본값: `false`)
  — 빈 속성을 축소합니다: `class=""` 대신 `class`를 얻습니다; 
  SVG 공간에서는 사용되지 않습니다; 
  boolean 속성(예: `hidden`)은 항상 축소됩니다
* `omitOptionalTags` (`boolean`, 기본값: `false`)
  — 선택적 여는 태그와 닫는 태그를 생략합니다; 
  예를 들어, `<ol><li>one</li><li>two</li></ol>`에서 두 개의 `</li>` 닫는 태그를 모두 생략할 수 있습니다. 
  첫 번째는 다른 `li`가 뒤따르기 때문에, 마지막은 뒤에 아무것도 없기 때문입니다; 
  SVG 공간에서는 사용되지 않습니다
* `preferUnquoted` (`boolean`, 기본값: `false`)
  — 더 적은 바이트로 표현될 경우 속성을 따옴표로 묶지 않습니다; 
  SVG 공간에서는 사용되지 않습니다
* `quote` (`'"'` 또는 `"'"`, 기본값: `'"'`)
  — 사용할 선호하는 따옴표
* `quoteSmart` (`boolean`, 기본값: `false`)
  — 더 적은 바이트로 표현될 경우 다른 따옴표를 사용합니다
* `space` (`'html'` 또는 `'svg'`, 기본값: `'html'`)
  — 문서가 있는 공간; 
  HTML 공간에서 `<svg>` 요소가 발견되면 이 패키지는 이미 자동으로 SVG 공간으로 전환했다가 다시 돌아옵니다
* `tightAttributes` (`boolean`, 기본값: `false`)
  — 가능한 경우 속성을 공백 없이 함께 결합하여 바이트를 절약합니다: 
  `class="a b" title="c d"` 대신 `class="a b"title="c d"`를 얻습니다; 
  SVG 공간에서는 사용되지 않습니다
* `tightCommaSeparatedLists` (`boolean`, 기본값: `false`)
  — 알려진 쉼표로 구분된 속성 값을 오른쪽에 패딩(`␠`는 공백을 나타냅니다)하는 대신 단순히 쉼표(`,`)로만 결합합니다
* `tightDoctype` (`boolean`, 기본값: `false`)
  — 바이트를 절약하기 위해 doctype에서 불필요한 공백을 제거합니다: `<!doctype html>` 대신 `<!doctypehtml>`
* `tightSelfClosing` (`boolean`, 기본값: `false`).
  — 자체 닫힘 요소를 닫을 때 추가 공백을 사용하지 않습니다: `<img />` 대신 `<img/>`; 
  `closeSelfClosing: true` 또는 `closeEmptyElements: true`인 경우에만 사용됩니다
* `upperDoctype` (`boolean`, 기본값: `false`).
  — `<!doctype…` 대신 `<!DOCTYPE…`를 사용합니다; XHTML을 제외하고는 무의미합니다
* `voids` (`Array<string>`, 기본값:
  [`html-void-elements`][html-void-elements])
  — 닫는 태그 없이 직렬화할 요소의 태그 이름; SVG 공간에서는 사용되지 않습니다

## 구문

HTML은 WHATWG HTML(현재 표준)에 따라 직렬화되며, 이는 모든 브라우저에서도 따르고 있습니다.

## 구문 트리

rehype에서 사용되는 구문 트리 형식은 [hast][]입니다.

## 타입

이 패키지는 [TypeScript][]로 완전히 타입이 지정되어 있습니다.
추가로 [`CharacterReferences`][api-character-references]와
[`Options`][api-options] 타입을 내보냅니다.

## 호환성

unified 그룹에서 유지 관리하는 프로젝트들은 Node.js의 유지 관리 버전과 호환됩니다.

새로운 메이저 릴리스를 할 때, 우리는 Node.js의 유지 관리되지 않는 버전에 대한 지원을 중단합니다.
이는 현재 릴리스 라인인 `rehype-stringify@^10`을 Node.js 16과 호환되도록 유지하려 노력한다는 의미입니다.

## 보안

**rehype**는 HTML을 다루기 때문에, rehype의 부적절한 사용은 [크로스 사이트 스크립팅(XSS)][xss] 공격에 취약할 수 있습니다.
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

[rehype-parse]: ../rehype-parse/readme-ko.md

[rehype-core]: ../rehype/readme-ko.md

[rehype-sanitize]: https://github.com/rehypejs/rehype-sanitize

[rehype-format]: https://github.com/rehypejs/rehype-format

[rehype-minify]: https://github.com/rehypejs/rehype-minify

[rehype-dom-stringify]: https://github.com/rehypejs/rehype-dom/tree/main/packages/rehype-dom-stringify

[hast-util-to-html]: https://github.com/syntax-tree/hast-util-to-html

[xast-util-to-xml]: https://github.com/syntax-tree/xast-util-to-xml

[html-void-elements]: https://github.com/wooorm/html-void-elements

<!-- 할 일: `remark-rehype` 링크가 릴리스되면 사용? -->

[raw]: https://github.com/syntax-tree/mdast-util-to-hast?tab=readme-ov-file#raw

[api-character-references]: #characterreferences

[api-options]: #options

[api-rehype-stringify]: #unifieduserehypestringify-options