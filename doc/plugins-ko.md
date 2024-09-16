![rehype][logo]

# 플러그인

**rehype**는 플러그인을 사용하여 HTML을 변환하는 도구입니다.
rehype 생태계에 대한 정보는 [monorepo readme][rehype]를 참조하세요.
이 페이지는 기존 플러그인을 나열합니다.

## 목차

* [플러그인 목록](#플러그인-목록)
* [유틸리티 목록](#유틸리티-목록)
* [플러그인 사용하기](#플러그인-사용하기)
* [플러그인 만들기](#플러그인-만들기)

## 플러그인 목록

생태계에서 가장 주목할 만한 프로젝트는 [`awesome-rehype`][awesome-rehype]를 참조하세요.
더 많은 플러그인은 GitHub에서 [`rehype-plugin` 토픽][topic]으로 태그된 것을 찾을 수 있습니다.

> 💡 **팁**: rehype 플러그인은 HTML과 함께 작동하고 **remark** 플러그인은 마크다운과 함께 작동합니다.
> 더 많은 플러그인은 remark의 [플러그인 목록][remark-plugins]을 참조하세요.

플러그인 목록:

* [`rehype-accessible-emojis`](https://github.com/GaiAma/Coding4GaiAma/tree/HEAD/packages/rehype-accessible-emojis)
  — role & aria-label을 추가하여 이모지를 접근 가능하게 만듦
* [`rehype-annotate`](https://github.com/baldurbjarnason/rehype-annotate)
  — W3C 스타일 주석 추가
* [`rehype-attr`](https://github.com/jaywcjlove/rehype-attr)
  — 속성을 추가하기 위한 새로운 마크다운 구문
* [`rehype-auto-ads`](https://github.com/robot-Inventor/rehype-auto-ads)
  — 지정된 수의 단락마다 광고 코드 삽입
* [`rehype-autolink-headings`](https://github.com/rehypejs/rehype-autolink-headings)
  — 제목에 링크 추가
* [`rehype-callouts`](https://github.com/lin-stephanie/rehype-callouts)
  — 블록 인용 기반 콜아웃(admonitions/alerts) 렌더링
* [`rehype-citation`](https://github.com/timlrx/rehype-citation)
  — bibtex에서 인용 및 참고문헌 추가
* [`rehype-class-names`](https://github.com/riderjensen/rehype-class-names)
  — 선택자별로 클래스 추가
* [`rehype-color-chips`](https://github.com/shreshthmohan/rehype-color-chips)
  — 색상 코드가 있는 인라인 코드 블록에 색상 칩 추가
* [`rehype-components`](https://github.com/marekweb/rehype-components)
  — 컴포넌트(사용자 정의 요소) 렌더링
* [`rehype-concat-css-style`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-concat-css-style)
  — `<style>`을 함께 연결
* [`rehype-concat-javascript`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-concat-javascript)
  — `<script>`를 함께 연결
* [`rehype-css-to-top`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-css-to-top)
  — `<link>`를 `<head>`로 이동
* [`rehype-document`](https://github.com/rehypejs/rehype-document)
  — 문서로 래핑
* [`rehype-dom-parse`](https://github.com/rehypejs/rehype-dom/tree/main/packages/rehype-dom-parse)
  — 브라우저에서 HTML 입력 구문 분석 지원 추가
* [`rehype-dom-stringify`](https://github.com/rehypejs/rehype-dom/tree/main/packages/rehype-dom-stringify)
  — 브라우저에서 HTML 직렬화 지원 추가
* [`rehype-external-links`](https://github.com/rehypejs/rehype-external-links)
  — 외부 링크에 rel (및 target) 추가
* [`rehype-extract-meta`](https://github.com/gorango/rehype-extract-meta)
  — HTML 문서에서 메타 데이터 추출
* [`rehype-figure`](https://github.com/josestg/rehype-figure)
  — 이미지에서 figure와 caption 지원
* [`rehype-format`](https://github.com/rehypejs/rehype-format)
  — HTML 형식 지정
* [`rehype-highlight`](https://github.com/rehypejs/rehype-highlight)
  — `lowlight`를 통해 Highlight.js로 코드 블록 구문 강조
* [`rehype-highlight-code-block`](https://github.com/mapbox/rehype-highlight-code-block)
  — 제공하는 함수로 코드 블록 구문 강조
* [`rehype-highlight-code-lines`](https://github.com/ipikuka/rehype-highlight-code-lines)
  — 코드 블록에 줄 번호 추가; 원하는 코드 줄 강조 허용
* [`rehype-infer-description-meta`](https://github.com/rehypejs/rehype-infer-description-meta)
  — 문서 내용에서 파일 메타데이터 추론
* [`rehype-infer-reading-time-meta`](https://github.com/rehypejs/rehype-infer-reading-time-meta)
  — 문서에서 읽기 시간을 파일 메타데이터로 추론
* [`rehype-infer-title-meta`](https://github.com/rehypejs/rehype-infer-title-meta)
  — 문서의 주 제목에서 파일 메타데이터 추론
* [`rehype-inline`](https://github.com/marko-knoebl/rehype-inline)
  — JS, CSS 및 이미지 파일 인라인화
* [`rehype-inline-svg`](https://github.com/JS-DevTools/rehype-inline-svg)
  — SVG 이미지 인라인화 및 최적화
* [`rehype-ignore`](https://github.com/jaywcjlove/rehype-ignore)
  — HTML 주석을 통해 콘텐츠 표시 무시
* [`rehype-jargon`](https://github.com/freesewing/freesewing/tree/develop/packages/rehype-jargon)
  — 전문 용어에 대한 정의 삽입
* [`rehype-javascript-to-bottom`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-javascript-to-bottom)
  — `<script>`를 `<body>` 끝으로 이동
* [`rehype-join-line`](https://github.com/unix/rehype-join-line)
  — 중국어 문단의 줄 바꿈 해결
* [`rehype-katex`](https://github.com/remarkjs/remark-math/tree/main/packages/rehype-katex)
  — KaTeX로 수학 렌더링
* [`rehype-lodash-template`](https://github.com/viktor-yakubiv/rehype-lodash-template)
  — 템플릿 문자열을 사전의 값으로 대체
* [`rehype-mathjax`](https://github.com/remarkjs/remark-math/tree/main/packages/rehype-mathjax)
  — MathJax로 수학 렌더링
* [`rehype-mermaidjs`](https://github.com/remcohaszing/rehype-mermaidjs)
  — mermaid 다이어그램 렌더링
* [`rehype-meta`](https://github.com/rehypejs/rehype-meta)
  — 문서의 head에 메타데이터 추가
* [`rehype-minify`](https://github.com/rehypejs/rehype-minify)
  — HTML 최소화
* [`rehype-minify-attribute-whitespace`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-minify-attribute-whitespace)
  — 속성의 공백 최소화
* [`rehype-minify-css-style`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-minify-css-style)
  — `<style>`의 CSS 최소화
* [`rehype-minify-enumerated-attribute`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-minify-enumerated-attribute)
  — 열거형 속성 최소화
* [`rehype-minify-event-handler`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-minify-event-handler)
  — 이벤트 핸들러 속성 최소화
* [`rehype-minify-javascript-script`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-minify-javascript-script)
  — `<script>`의 JS 최소화
* [`rehype-minify-javascript-url`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-minify-javascript-url)
  — `javascript:` URL 최소화
* [`rehype-minify-json-script`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-minify-json-script)
  — `<script>`의 JSON 최소화
* [`rehype-minify-language`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-minify-language)
  —  `lang` 속성 최소화
* [`rehype-minify-media-attribute`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-minify-media-attribute)
  — `media` 속성 최소화
* [`rehype-minify-meta-color`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-minify-meta-color)
  — 테마 색상 `<meta>`의 `content` 최소화
* [`rehype-minify-meta-content`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-minify-meta-content)
  — `<meta>`의 `content` 최소화
* [`rehype-minify-style-attribute`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-minify-style-attribute)
  — `style` 속성 최소화
* [`rehype-minify-url`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-minify-url)
  — URL 속성 최소화
* [`rehype-minify-whitespace`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-minify-whitespace)
  — 요소 간 공백 최소화
* [`rehype-normalize-attribute-value-case`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-normalize-attribute-value-case)
  — 속성 값의 대소문자 정규화
* [`rehype-partials`](https://github.com/mrzmmr/rehype-partials)
  — rehype를 위한 부분 지원
* [`rehype-picture`](https://github.com/rehypejs/rehype-picture)
  — 이미지를 `<picture>`로 래핑
* [`rehype-postcss`](https://github.com/viktor-yakubiv/rehype-postcss)
  — `<style>` 노드와 `style` 속성이 있는 요소에서 PostCSS 실행
* [`rehype-prevent-favicon-request`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-prevent-favicon-request)
  — 빈 `favicon.ico`를 설정하여 요청 방지
* [`rehype-prism`](https://github.com/mapbox/rehype-prism)
  — `refractor`를 통한 Prism으로 구문 강조
* [`rehype-prism-plus`](https://github.com/timlrx/rehype-prism-plus)
  — 추가 기능이 있는 `refractor`를 통한 Prism으로 구문 강조
* [`rehype-raw`](https://github.com/rehypejs/rehype-raw)
  — 트리를 다시 구문 분석 (및 원시 노드)
* [`rehype-react`](https://github.com/rehypejs/rehype-react)
  — React로 컴파일
* [`rehype-remark`](https://github.com/rehypejs/rehype-remark)
  — remark 지원
* [`rehype-remove-comments`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-remove-comments)
  — 주석 제거
* [`rehype-remove-duplicate-attribute-values`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-remove-duplicate-attribute-values)
  — 중복 속성 값 제거
* [`rehype-remove-empty-attribute`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-remove-empty-attribute)
  — 빈 속성 제거
* [`rehype-remove-external-script-content`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-remove-external-script-content)
  — `src`가 있는 `<script>`의 내용 제거
* [`rehype-remove-images`](https://github.com/iloveitaly/rehype-remove-images)
* [`rehype-remove-meta-http-equiv`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-remove-meta-http-equiv)
  — 특정 `http-equiv`를 더 짧은 형태로 대체
* [`rehype-remove-script-type-javascript`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-remove-script-type-javascript)
  — JS `<script>`에서 `type` 및 `language` 제거
* [`rehype-remove-style-type-css`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-remove-style-type-css)
  — CSS `<style>` 및 `<link>`에서 `type` 제거
* [`rehype-remove-unused-css`](https://github.com/nzt/rehype-remove-unused-css)
  — 사용되지 않는 CSS 제거
* [`rehype-resolution`](https://github.com/michaelnisi/rehype-resolution)
  — 이미지에 해상도 `srcset` 주입
* [`rehype-retext`](https://github.com/rehypejs/rehype-retext)
  — retext 지원
* [`rehype-rewrite`](https://github.com/jaywcjlove/rehype-rewrite)
  — rehype로 요소 다시 작성
* [`rehype-sanitize`](https://github.com/rehypejs/rehype-sanitize)
  — HTML 살균
* [`rehype-scroll-to-top`](https://github.com/benjamincharity/rehype-scroll-to-top)
  — 사용자 정의 가능한 "맨 위로 스크롤" 및 "맨 아래로 스크롤" 링크
* [`rehype-section`](https://github.com/agentofuser/rehype-section)
  — 제목과 그 내용을 중첩된 `<section>` 요소로 래핑
* [`rehype-sectionize`](https://github.com/hbsnow/rehype-sectionize)
  — 제목과 그 내용을 속성이 있는 중첩된 `<section>` 요소로 래핑
* [`rehype-semantic-images`](https://github.com/benjamincharity/rehype-semantic-images)
  — HTML 이미지를 의미론적 요소와 사용자 정의 가능한 기능으로 강화
* [`rehype-shift-heading`](https://github.com/rehypejs/rehype-shift-heading)
  — 제목의 순위 변경
* [`rehype-shiki`](https://github.com/rsclarke/rehype-shiki)
  — Shiki로 코드 블록 구문 강조
* [`rehype-slots`](https://github.com/marekweb/rehype-slots)
  — 슬롯 요소를 주입된 콘텐츠로 대체
* [`rehype-slug`](https://github.com/rehypejs/rehype-slug)
  — 제목에 `id` 추가
* [`rehype-slug-custom-id`](https://github.com/unicorn-utterances/rehype-slug-custom-id)
  — 제목에 `id` 추가, `{#custom-ids}` 또한 지원
* [`rehype-sort-attribute-values`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-sort-attribute-values)
  — 속성 값 정렬
* [`rehype-sort-attributes`](https://github.com/rehypejs/rehype-minify/tree/main/packages/rehype-sort-attributes)
  — 속성 정렬
* [`rehype-sort-tailwind-classes`](https://github.com/bitcrowd/rehype-sort-tailwind-classes)
  — tailwind 클래스 정렬
* [`rehype-starry-night`](https://github.com/rehypejs/rehype-starry-night)
  — [`starry-night`](https://github.com/wooorm/starry-night)를 사용하여 코드에 구문 강조 적용
* [`rehype-svgo`](https://github.com/TomerAberbach/rehype-svgo)
  — [SVGO](https://github.com/svg/svgo)를 사용하여 인라인 SVG 최적화
* [`rehype-template`](https://github.com/nzt/rehype-template)
  — 템플릿 리터럴로 콘텐츠 래핑
* [`rehype-toc`](https://github.com/JS-DevTools/rehype-toc)
  — 페이지에 목차(TOC) 추가
* [`rehype-truncate`](https://github.com/luk707/rehype-truncate)
  — HTML 구조를 보존하면서 HTML 자르기
* [`rehype-twemojify`](https://github.com/cliid/rehype-twemojify)
  — 이모지 단축 코드를 twemoji로 변환
* [`rehype-twoslash`](https://github.com/rehypejs/rehype-twoslash)
  — `twoslash`로 JavaScript 및 TypeScript 코드 처리 및 `starry-night`로 강조 표시
* [`rehype-urls`](https://github.com/brechtcs/rehype-urls)
  — `href` 및 `src` 속성의 URL 다시 작성
* [`rehype-url-inspector`](https://github.com/JS-DevTools/rehype-url-inspector)
  — 문서 어디에서나 URL 검사, 유효성 검사 또는 다시 작성
* [`rehype-video`](https://jaywcjlove.github.io/rehype-video)
  — 개선된 비디오 구문: `.mp4` 및 `.mov`에 대한 링크를 비디오로 변환
* [`rehype-webparser`](https://github.com/Prettyhtml/prettyhtml/tree/HEAD/packages/rehype-webparser)
  — 덜 엄격한 HTML 파서
* [`rehype-widont`](https://github.com/radiojhero/rehype-widont)
  — 단일 단어로 된 줄 방지
* [`rehype-wrap`](https://github.com/mrzmmr/rehype-wrap)
  — 선택된 요소를 주어진 요소로 래핑
* [`rehype-wrap-all`](https://github.com/florentb/rehype-wrap-all)
  — 모든 일치하는 요소를 주어진 요소로 래핑

## 유틸리티 목록

구문 트리와 함께 작동하는 유틸리티 목록은 [hast][hast-util]를 참조하세요.
hast 및 기타 구문 트리와 함께 작동하는 다른 유틸리티는 [unist][unist-util]를 참조하세요.
마지막으로, 가상 파일과 함께 작동하는 유틸리티 목록은 [vfile][vfile-util]을 참조하세요.

## 플러그인 사용하기

프로그래밍 방식으로 플러그인을 사용하려면 [`use()`][unified-use] 함수를 호출하세요.
`rehype-cli`로 플러그인을 사용하려면 [`--use` 플래그][unified-args-use]를 전달하거나
[환경설정 파일][config-file-use]에 지정하세요.

## 플러그인 만들기

플러그인을 만들려면 먼저 [플러그인 개념][unified-plugins]에 대해 읽어보세요.
그런 다음 ["unified로 플러그인 만들기" 가이드][guide]를 읽어보세요.
마지막으로, 만들려는 것과 유사해 보이는 기존 플러그인 중 하나를 가져와서 거기서부터 작업하세요.
막히면 [discussions][]에서 도움을 받을 수 있습니다.

`'rehype-'` 접두사가 붙은 이름을 선택해야 합니다(예: `rehype-format`).
만드는 것이 `rehype().use()`와 함께 작동하지 않는다면 **`rehype-` 접두사를 사용하지 마세요**: 
그것은 "플러그인"이 아니며 사용자를 혼란스럽게 할 것입니다.
hast와 함께 작동한다면 `'hast-util-'`을 사용하고, 모든 unist 트리와 함께 작동한다면
`unist-util-`을 사용하고, 가상 파일과 함께 작동한다면 `vfile-`을 사용하세요.

패키지에서 플러그인을 노출하려면 기본 내보내기를 사용하고, `package.json`에 `rehype-plugin`
키워드를 추가하고, GitHub의 저장소에 `rehype-plugin` 토픽을 추가하고, 이 페이지에
플러그인을 추가하기 위한 풀 리퀘스트를 만드세요!

<!--정의:-->

[logo]: https://raw.githubusercontent.com/rehypejs/rehype/cb624bd/logo.svg?sanitize=true

[rehype]: https://github.com/rehypejs/rehype

[awesome-rehype]: https://github.com/rehypejs/awesome-rehype

[topic]: https://github.com/topics/rehype-plugin

[hast-util]: https://github.com/syntax-tree/hast#list-of-utilities

[unist-util]: https://github.com/syntax-tree/unist#unist-utilities

[vfile-util]: https://github.com/vfile/vfile#utilities

[unified-use]: https://github.com/foreverfl/unified/blob/main/readme-ko.md#processoruseplugin-options

[unified-args-use]: https://github.com/unifiedjs/unified-args#--use-plugin

[config-file-use]: https://github.com/unifiedjs/unified-engine/blob/main/doc/configure.md#plugins

[unified-plugins]: https://github.com/unifiedjs/unified#plugin

[guide]: https://unifiedjs.com/learn/guide/create-a-plugin/

[discussions]: https://github.com/rehypejs/rehype/discussions

[remark-plugins]: https://github.com/remarkjs/remark/blob/main/doc/plugins.md#list-of-plugins
