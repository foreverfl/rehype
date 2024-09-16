# rehype-cli

[![Build][build-badge]][build]
[![Coverage][coverage-badge]][coverage]
[![Downloads][downloads-badge]][downloads]
[![Sponsors][sponsors-badge]][collective]
[![Backers][backers-badge]][collective]
[![Chat][chat-badge]][chat]

**[rehype][]**를 사용하여 HTML 파일을 검사하고 변경하기 위한 명령줄 인터페이스입니다.

## 목차

* [이게 무엇인가요?](#이게-무엇인가요)
* [언제 사용해야 하나요?](#언제-사용해야-하나요)
* [설치](#설치)
* [사용법](#사용법)
* [CLI](#cli)
* [호환성](#호환성)
* [보안](#보안)
* [기여하기](#기여하기)
* [후원하기](#후원하기)
* [라이선스](#라이선스)

## 이게 무엇인가요?

이 패키지는 터미널이나 npm 스크립트 등에서 HTML 파일을 검사하고 변경하는 데 사용할 수 있는 명령줄 인터페이스(CLI)입니다.
이 CLI는 HTML을 구조화된 데이터, 특히 AST(추상 구문 트리)로 다루는 플러그인 생태계인 rehype를 기반으로 구축되었습니다.
기존 플러그인 중에서 선택하거나 직접 만들 수 있습니다.

rehype 생태계에 대한 정보는 [모노레포의 readme][rehype]를 참조하세요.

## 언제 사용해야 하나요?

프로젝트의 HTML 파일을 명령줄에서 작업하고 싶을 때 이 패키지를 사용할 수 있습니다.
`rehype-cli`는 많은 옵션을 가지고 있고 여러 플러그인과 결합할 수 있어서 원하는 작업을 수행할 수 있을 것입니다.
그렇지 않다면 스크립트에서 [`rehype`][rehype-core] 자체를 수동으로 사용할 수 있습니다.

## 설치

이 패키지는 [ESM 전용][esm]입니다.
Node.js(버전 16 이상)에서는 [npm][]으로 설치하세요:

```sh
npm install rehype-cli
```

## 사용법

[`rehype-format`][rehype-format]을 사용하여 `index.html` 형식 지정:

```sh
rehype index.html --use rehype-format --output
```

## CLI

`rehype-cli`의 인터페이스는 다음과 같이 도움말 페이지(`rehype --help`)에 설명되어 있습니다:

```txt
Usage: rehype [options] [path | glob ...]

  rehype를 사용하여 HTML을 처리하는 CLI

Options:

      --[no-]color                        보고서에 색상 지정 (기본값: 켜짐)
      --[no-]config                       구성 파일 검색 (기본값: 켜짐)
  -e  --ext <extensions>                  확장자 지정
      --file-path <path>                  처리할 경로 지정
  -f  --frail                             경고 시 1로 종료
  -h  --help                              사용법 정보 출력
      --[no-]ignore                       무시 파일 검색 (기본값: 켜짐)
  -i  --ignore-path <path>                무시 파일 지정
      --ignore-path-resolve-from cwd|dir  `ignore-path`의 패턴을 해당 디렉토리 또는 cwd에서 해석
      --ignore-pattern <globs>            무시 패턴 지정
      --inspect                           형식화된 구문 트리 출력
  -o  --output [path]                     출력 위치 지정
  -q  --quiet                             경고와 오류만 출력
  -r  --rc-path <path>                    구성 파일 지정
      --report <reporter>                 리포터 지정
  -s  --setting <settings>                설정 지정
  -S  --silent                            오류만 출력
      --silently-ignore                   무시된 파일이 주어졌을 때 실패하지 않음
      --[no-]stdout                       stdout에 쓰기 지정 (기본값: 켜짐)
  -t  --tree                              입력과 출력을 구문 트리로 지정
      --tree-in                           입력을 구문 트리로 지정
      --tree-out                          구문 트리 출력
  -u  --use <plugins>                     플러그인 사용
      --verbose                           메시지에 대한 추가 정보 보고
  -v  --version                           버전 번호 출력
  -w  --watch                             변경 사항을 감시하고 재처리

Examples:

  # `input.html` 처리
  $ rehype input.html -o output.html

  # 파이프
  $ rehype < input.html > output.html

  # 모든 해당 파일 다시 쓰기
  $ rehype . -o
```

이 모든 옵션에 대한 자세한 정보는 작업을 수행하는 [`unified-args`][unified-args]에서 확인할 수 있습니다.
`rehype-cli`는 다음과 같이 사전 구성된 `unified-args`입니다:

* `rehype-` 플러그인 로드
* HTML 확장자 검색 (`.html`, `.htm`, `.xht`, `.xhtml`)
* [`.rehypeignore` 파일][ignore-file]에서 찾은 경로 무시
* [`.rehyperc`, `.rehyperc.js` 등 파일][config-file]에서 구성 로드
* [`package.json` 파일의 `rehype` 필드][config-file]에서 구성 사용

## 호환성

unified 그룹에서 유지 관리하는 프로젝트들은 Node.js의 유지 관리 버전과 호환됩니다.

새로운 메이저 릴리스를 할 때, 우리는 Node.js의 유지 관리되지 않는 버전에 대한 지원을 중단합니다.
이는 현재 릴리스 라인인 `rehype-cli@^12`를 Node.js 16과 호환되도록 유지하려 노력한다는 의미입니다.

## 보안

**rehype**는 HTML을 다루기 때문에, HTML의 부적절한 사용은 [크로스 사이트 스크립팅(XSS)][xss] 공격에 취약할 수 있습니다.
따라서 rehype의 사용도 안전하지 않을 수 있습니다.
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

[esm]: https://gist.github.com/

[npm]: https://docs.npmjs.com/cli/install

[rehype]: https://github.com/rehypejs/rehype

[xss]: https://en.wikipedia.org/wiki/Cross-site_scripting

[rehype-core]: ../rehype/

[rehype-format]: https://github.com/rehypejs/rehype-format

[rehype-sanitize]: https://github.com/rehypejs/rehype-sanitize

[config-file]: https://github.com/unifiedjs/unified-engine/blob/main/readme.md#config-files

[ignore-file]: https://github.com/unifiedjs/unified-engine/blob/main/readme.md#ignore-files

[unified-args]: https://github.com/unifiedjs/unified-args#cli
