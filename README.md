# YTAlertDiscord

YouTube 채널의 새 영상을 자동으로 감지하여 Discord로 알림을 보내는 GitHub Actions 워크플로우.

## 특징

- 사용자 서버 없이 GitHub Actions로만 동작
- 특정 시간마다 자동 실행 (cron 수정으로 간격 조정 가능) 또는 수동 워크플로우 실행 가능
- 채널 ID와 영상 정보는 모두 Secrets에 보관하고, 저장이 필요한 영상 정보 값은 해시(+salt) 기반 상태 관리로 퍼블릭 레포지토리에서도 어떤 채널을 구독 중인지 외부에서 알 수 없음

## 설치

1. 이 레포지토리를 fork 또는 복제
2. GitHub 레포지토리 Settings > Secrets and variables > Actions에서 다음 Secrets 추가:
   - `CHANNEL_ID`: YouTube 채널 ID (UCxxxxxxxxxxxxxxxxxxxxx 형식)
   - `DISCORD_WEBHOOK_URL`: Discord Webhook URL
   - `HASH_SALT`: HASH SALT 값. 랜덤 문자열 권장 (예: `openssl rand -hex 16` 실행 결과)

## 설정

워크플로우 파일 상단의 환경 변수 수정 가능:

- `cron`: 실행 주기 (기본: 2시간마다)
- `MAX_EXTRACT`: RSS에서 확인할 영상 개수 (기본: 5)
- `MAX_STORED`: 저장할 해시 개수 (기본: 100)

## 작동 방식

1. YouTube RSS 피드에서 최신 영상 목록 가져오기
2. 각 영상 ID를 해시(+salt)하여 비가역적으로 변환
3. 해시 값을 state.json과 비교하여 새 영상 감지
4. 새 영상 발견 시 Discord로 알림 전송
5. 업데이트된 해시 목록을 state.json에 저장

## 라이센스

[MIT License](/LICENSE)
