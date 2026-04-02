# Slack 알람 설정 가이드

PR 생성 및 이슈 완료 시 팀 Slack 채널로 알람을 받을 수 있습니다.

## 알람 시점

| 커맨드 | 채널 | 메시지 예시 |
|--------|------|------------|
| `/ls:pr` | 팀 채널 | `[ADE-24] 로그 레벨 분리 — PR 리뷰 요청: https://github.com/.../pull/42` |
| `/ls:done` | 팀 채널 | `[ADE-24] 로그 레벨 분리 — 완료` |

작업 시작(`/ls:start`)은 본인이 직접 실행하는 행위이므로 알람을 전송하지 않습니다.

---

## Step 1: Slack Incoming Webhook 생성

1. [Slack API 앱 관리 페이지](https://api.slack.com/apps)로 이동
2. **Create New App** → **From scratch** 선택
3. App 이름 입력 (예: `linear-skill-bot`), 워크스페이스 선택 후 **Create App**
4. 좌측 메뉴 **Features → Incoming Webhooks** 클릭
5. **Activate Incoming Webhooks** 토글 → **On**
6. **Add New Webhook to Workspace** 클릭
7. 팀 공용 채널 선택 후 **허용**
8. 생성된 Webhook URL 복사

```
https://hooks.slack.com/services/<WORKSPACE_ID>/<APP_ID>/<TOKEN>
```

---

## Step 2: /ls:setup에서 Webhook URL 등록

`/ls:setup` 실행 시 팀 Webhook URL을 입력합니다:

```
팀 Slack Webhook URL을 입력하세요 (PR 생성·완료 알람, 엔터로 스킵):
> https://hooks.slack.com/services/...
```

저장 위치: `~/.config/linear/config.json`

```json
{
  "api_key": "lin_api_xxxxx",
  "slack_team_webhook": "https://hooks.slack.com/services/..."
}
```

---

## Webhook URL 변경 또는 삭제

- **변경**: `/ls:setup`을 다시 실행하여 새 URL 입력
- **삭제**: `~/.config/linear/config.json`에서 `slack_team_webhook` 값을 빈 문자열로 수정

값이 비어 있으면 알람이 조용히 스킵됩니다.

---

## 팀 도입 시 권장 설정

팀원 전원이 동일한 팀 webhook URL을 `/ls:setup`에서 입력하면  
PR 생성·완료 알람이 팀 채널에 자동으로 집약됩니다.
