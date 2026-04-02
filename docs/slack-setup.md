# Slack 알람 설정 가이드

`/ls:start`, `/ls:pr`, `/ls:done` 실행 시 Slack 채널로 알람을 받을 수 있습니다.

## 알람 시점

| 커맨드 | 알람 메시지 예시 |
|--------|----------------|
| `/ls:start` | `[ADE-24] 작업 시작 — ade-24-log-level-separation` |
| `/ls:pr` | `[ADE-24] PR 생성 — https://github.com/org/repo/pull/42` |
| `/ls:done` | `[ADE-24] 완료 ✓` |

---

## Step 1: Slack Incoming Webhook 생성

1. [Slack API 앱 관리 페이지](https://api.slack.com/apps)로 이동
2. **Create New App** → **From scratch** 선택
3. App 이름 입력 (예: `linear-skill-bot`), 워크스페이스 선택 후 **Create App**
4. 좌측 메뉴 **Features → Incoming Webhooks** 클릭
5. **Activate Incoming Webhooks** 토글 → **On**
6. 하단 **Add New Webhook to Workspace** 클릭
7. 알람을 받을 채널 선택 후 **허용**
8. 생성된 Webhook URL 복사

```
https://hooks.slack.com/services/<WORKSPACE_ID>/<APP_ID>/<TOKEN>
```

---

## Step 2: /ls:setup에서 Webhook URL 등록

프로젝트 루트에서 `/ls:setup`을 실행하면 마지막 단계에서 Webhook URL을 입력받습니다:

```
Slack Webhook URL을 입력하세요 (선택사항, 엔터로 스킵):
> https://hooks.slack.com/services/...
```

입력하면 `~/.config/linear/config.json`에 저장됩니다:

```json
{
  "api_key": "lin_api_xxxxx",
  "slack_webhook": "https://hooks.slack.com/services/..."
}
```

---

## Step 3: 동작 확인

`/ls:start ADE-24`를 실행하면 지정한 채널에 다음과 같은 메시지가 도착합니다:

```
[ADE-24] 작업 시작 — ade-24-log-level-separation
```

---

## Webhook URL 변경 또는 삭제

- **변경**: `/ls:setup`을 다시 실행하여 새 URL 입력
- **삭제**: `~/.config/linear/config.json`에서 `slack_webhook` 값을 빈 문자열로 수정

```json
{
  "api_key": "lin_api_xxxxx",
  "slack_webhook": ""
}
```

Webhook URL이 비어 있으면 알람이 조용히 스킵됩니다.
