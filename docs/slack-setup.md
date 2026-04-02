# Slack 알람 설정 가이드

알람을 **개인 채널**과 **팀 채널**로 분리하여 노이즈를 최소화합니다.

## 알람 분리 원칙

| webhook | 이벤트 | 수신 대상 | 이유 |
|---------|--------|----------|------|
| 개인 webhook | `/ls:start` — 작업 시작 | 본인만 | 팀에는 노이즈 |
| 팀 webhook | `/ls:pr` — PR 생성 | 팀 전체 | 리뷰 요청 시점 |
| 팀 webhook | `/ls:done` — 이슈 완료 | 팀 전체 | 진행 상황 공유 |

---

## Step 1: Slack Incoming Webhook 생성

개인 채널용과 팀 채널용, 각각 Webhook URL을 생성합니다.

1. [Slack API 앱 관리 페이지](https://api.slack.com/apps)로 이동
2. **Create New App** → **From scratch** 선택
3. App 이름 입력 (예: `linear-skill-bot`), 워크스페이스 선택 후 **Create App**
4. 좌측 메뉴 **Features → Incoming Webhooks** 클릭
5. **Activate Incoming Webhooks** 토글 → **On**
6. **Add New Webhook to Workspace** 클릭
7. 채널 선택 후 **허용** — 개인용은 본인 DM, 팀용은 팀 공용 채널 선택
8. 생성된 Webhook URL 복사

```
https://hooks.slack.com/services/<WORKSPACE_ID>/<APP_ID>/<TOKEN>
```

---

## Step 2: /ls:setup에서 Webhook URL 등록

`/ls:setup` 실행 시 두 개의 Webhook URL을 순서대로 입력합니다:

```
개인 Slack Webhook URL을 입력하세요 (작업 시작 알람, 엔터로 스킵):
> https://hooks.slack.com/services/...   ← 본인 DM 채널

팀 Slack Webhook URL을 입력하세요 (PR 생성·완료 알람, 엔터로 스킵):
> https://hooks.slack.com/services/...   ← 팀 공용 채널
```

둘 다 선택사항입니다. 필요한 것만 입력하고 나머지는 엔터로 스킵할 수 있습니다.

저장 위치: `~/.config/linear/config.json`

```json
{
  "api_key": "lin_api_xxxxx",
  "slack_personal_webhook": "https://hooks.slack.com/services/...",
  "slack_team_webhook": "https://hooks.slack.com/services/..."
}
```

---

## 알람 메시지 형식

| 이벤트 | 메시지 예시 |
|--------|------------|
| 작업 시작 (개인) | `[ADE-24] 작업 시작 — ade-24-log-level-separation` |
| PR 생성 (팀) | `[ADE-24] 로그 레벨 분리 — PR 리뷰 요청: https://github.com/.../pull/42` |
| 이슈 완료 (팀) | `[ADE-24] 로그 레벨 분리 — 완료` |

---

## Webhook URL 변경 또는 삭제

- **변경**: `/ls:setup`을 다시 실행하여 새 URL 입력
- **삭제**: `~/.config/linear/config.json`에서 해당 키 값을 빈 문자열로 수정

```json
{
  "api_key": "lin_api_xxxxx",
  "slack_personal_webhook": "",
  "slack_team_webhook": ""
}
```

값이 비어 있으면 해당 알람이 조용히 스킵됩니다.

---

## 팀 도입 시 권장 설정

| 역할 | 개인 webhook | 팀 webhook |
|------|------------|-----------|
| 팀장 | 본인 DM | 팀 채널 |
| 팀원 | 본인 DM (선택) | 팀 채널 |

팀원 전원이 동일한 팀 webhook URL을 사용하면 PR/완료 알람이 팀 채널에 집약됩니다.
