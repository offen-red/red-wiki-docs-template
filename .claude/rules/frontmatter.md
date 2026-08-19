---
paths:
  - "docs/**/*.md"
  - "docs/**/*.history.yaml"
---

# frontmatter · 원천 · 변경 이력

사람이 채우는 것은 `title` · `status` · `owner` 셋뿐이다(CLAUDE.md 5절). 아래는 **AI가 채우는** 나머지다.

## AI가 채우는 필드

```yaml
type: doc # doc | hub | screen | guide | skill (뷰어 렌더 분기)
summary: 한 줄 요약 # index.md·카탈로그에 실림
source: docs/_sources/<문서 폴더>/... # 어느 원천에서 왔나 (경로 또는 URL)
related: [경로, ...] # 백링크
domains: [도메인-id, ...] # registry.yaml 의 domains: 키
generated: true # AI가 초안을 썼고 사람이 아직 확인 안 함
```

`generated: true` 는 **사람이 검토하면 지운다.** 이게 "미검증" 표시 역할을 하며 액션이 명확하다(지우면 검토 완료). 별도 `verified` 필드는 쓰지 않는다 — 아무도 갱신하지 않아 반드시 거짓말이 된다.

## `mirror: notion` — 원본이 밖에 있는 문서

붙어 있으면 **노션이 원본인 사본이다.** 앱이 편집 버튼을 숨기고 `Notion` 뱃지와 [노션에서 열기] 를 띄운다. 여기서 고쳐도 다음 동기화에 덮인다.

그 문서를 고쳐 달라는 요청은 **노션에서 고치라고 답한다**(`docs/ops/tooling/notion-mirror.md`). 본문을 직접 수정하지 않는다.

## 원천은 `sources:` 한 곳에만

문서 보기가 제목 아래에 표로 그린다. **본문에 원천 표를 또 쓰지 않는다** — 같은 것이 두 곳에 있으면 반드시 어긋난다.

```yaml
sources:
  - id: req-excel
    kind: spreadsheet # notion | figma | spreadsheet | doc | github | url
    title: 요구사항 정의서
    file: docs/_sources/<문서 폴더>/요구사항.xlsx # 또는 url:
    role: authoritative # authoritative | reference
authoritative: req-excel
```

**`authoritative` 는 반드시 하나.** "어디가 최신이지?"를 없애는 장치다.

## 변경 이력

**`docs/product/` 문서만 쌓는다.** `ops/` 는 git log 로 충분하다.

**본문에 표로 쓰지 않는다.** 문서마다 짝이 되는 `<문서>.history.yaml` 에 쌓고 뷰어가 본문 아래에 이어 그린다. **짝 파일은 문서 옆에 둔다 — 이력이 붙어도 문서를 옮기지 않는다.** 경로가 생애 중간에 바뀌면 `registry.yaml`·상대링크가 한꺼번에 어긋난다. 에디터 트리에서는 `.vscode/settings.json` 의 fileNesting 이 짝 파일을 본문 아래로 접는다.

```
docs/product/overview.md      →  docs/product/overview.history.yaml
docs/…/GEN_0101/GEN_0101.md   →  docs/…/GEN_0101/GEN_0101.history.yaml
```

```yaml
- version: "v1.1"
  date: "2026-08-04"
  by: "수빈"
  change: "검수 2단계화"
  why: "오발행 방지"
```

**"왜"는 반드시 채운다.** "무엇"만 있고 "왜"가 없으면 위키로서 가치가 없다 — 그건 git log 가 이미 한다.

사람은 앱의 **편집** 폼 하단 `Change log` 에서 채운다(버전·날짜는 자동). 비워두고 저장하면 본문만 저장한다 — 오탈자 수정까지 이력을 남길 이유는 없다. AI 는 짝 파일을 직접 고친다.

`.md` 가 아니라서 카탈로그·사이드바에 걸리지 않는다.
