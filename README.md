# linear-skills

Linear 이슈 관리와 Git 브랜치/PR 워크플로우를 통합하는 Claude Code 플러그인.

이슈를 시작하면 Claude가 이슈 설명·수락기준·팀원 댓글을 세션 컨텍스트에 로드합니다.
이후 코드 작성·리뷰·PR 초안 생성까지 맥락을 유지합니다.

## 설치

```
/plugin marketplace add suslmk-lee/linear-skills
/plugin install ls@linear-skills
/reload-plugins
```

## 초기 설정

```
/ls:setup
```

Linear API 키, 팀, 워크플로우 상태 매핑, Slack Webhook을 설정합니다.

## 커맨드

| 커맨드 | 설명 |
|--------|------|
| `/ls:setup` | 초기 설정 |
| `/ls:start [ISSUE-KEY]` | 작업 시작 — 이슈 컨텍스트 주입 + 브랜치 생성 |
| `/ls:list` | 이슈 목록 (내 이슈 우선) |
| `/ls:scan` | 프로젝트 분석 후 이슈 일괄 등록 |
| `/ls:sub [PARENT-KEY]` | 특정 이슈에 서브이슈 등록 |
| `/ls:pr` | PR 초안 생성 + 확인 후 생성 |
| `/ls:done [ISSUE-KEY]` | 이슈 완료 처리 |
| `/ls:status` | 현재 작업 스냅샷 |

## 자연어 트리거

명시적 커맨드 없이도 자동으로 작동합니다:

- `"ADE-24 시작해줘"` → `/ls:start ADE-24`
- `"PR 올려줘"` → `/ls:pr`
- `"이슈 완료"` → `/ls:done`
- `"지금 뭐 하고 있어?"` → `/ls:status`

## 필요 조건

- `curl` — HTTP 요청
- `python3` — JSON 인코딩
- `gh` — GitHub PR 생성 (`/ls:pr` 커맨드용)
  - 설치: https://cli.github.com
  - 인증: `gh auth login`
- Linear API 키 — [Settings → API](https://linear.app/settings/api)
