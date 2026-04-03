# Slack 알람 설정 가이드

Linear의 공식 Slack 앱을 연동하면 이슈 상태 변경 시 자동으로 팀 채널에 알람이 발송됩니다.  
클라이언트 도구(Claude Code 스킬, 웹 UI, API 직접 호출 등)에 관계없이 동작합니다.

## 알람 시점

| 이벤트 | 채널 | 예시 메시지 |
|--------|------|------------|
| 이슈 상태 변경 (In Progress → Done 등) | 팀 채널 | Linear 앱 포맷으로 자동 발송 |
| PR 연결 (이슈 → In Review) | 팀 채널 | Linear 앱 포맷으로 자동 발송 |

---

## Step 1: Linear Slack 앱 설치

Linear 웹 앱에서:
1. **Settings → Integrations → Slack** 이동
2. **Connect** 클릭 → Slack 워크스페이스 인증 및 허용

---

## Step 2: 알람 채널 설정

채널 설정은 **팀(Team) 설정**에서 합니다 (글로벌 Integrations 페이지가 아님):

1. **Settings → Teams → [팀 이름] → Notifications** 이동
2. **Slack channel** 항목에 알람을 받을 채널 입력 (예: `#dev-alerts`)
3. 저장

---

## Step 3: 동작 확인

Linear에서 이슈 상태를 변경하면 설정한 Slack 채널에 알람이 자동 발송됩니다.

---

## 팀 도입 시 권장 설정

- **채널**: `#dev-linear` 또는 `#sprint-updates` 등 전용 채널 권장
- **이벤트**: "Issue state changed" 만 활성화하면 노이즈 최소화
- **팀 필터**: 작업 중인 팀만 선택하면 무관한 알람 차단

Linear Slack 앱은 워크스페이스 단위로 설치되므로, 한 번 설정하면 팀원 전원이 별도 설정 없이 알람을 받습니다.
