# Verified Baseline

검증 기준일: `2026-07-17`

이 파일은 영구적인 진실이 아니라 현재 프로젝트가 사용 중인 검증 스냅샷이다. 관련 코드를 변경할 때 공식 원문을 다시 확인한다.

## 1. 대상 환경

| 항목 | 기준 |
|---|---|
| 게임 | Palworld `v1.0.1` |
| 패치 날짜 | `2026-07-14` |
| 우선 플랫폼 | Steam Windows Dedicated Server |
| 인게임 기능 | 별도 GameBridge 모드 필요 |
| 서버 관리 | Palworld 공식 REST API |
| 방송 이벤트 | CHZZK Session API `DONATION` |

## 2. Palworld v1.0.1 확인 사항

PC Steam 공식 공지에서 확인한 수정 사항:

- 특정 조작 뒤 저장 데이터가 의도하지 않게 폐기될 수 있던 문제 수정
- 모닥불에 접촉한 뒤 연소 상태가 지속될 수 있던 문제 수정

해석 제한:

- 저장 손상 가능성이 완전히 제거됐다고 해석하지 않는다.
- 기존 모드가 자동으로 호환된다고 해석하지 않는다.
- 플랫폼별 추가 수정 사항은 해당 플랫폼 공식 공지 없이 공통 동작으로 가정하지 않는다.

## 3. Palworld REST 기준

개별 공식 REST 페이지에서 확인한 경로:

```text
GET  /v1/api/info
GET  /v1/api/players
GET  /v1/api/settings
GET  /v1/api/metrics
POST /v1/api/announce
POST /v1/api/kick
POST /v1/api/ban
POST /v1/api/unban
POST /v1/api/save
POST /v1/api/shutdown
POST /v1/api/stop
```

현재 구현 제한:

- `GET /settings`는 조회 전용
- 공식 REST에서 아이템 지급·팰 소환·효과 적용을 확인하지 못함
- REST는 HTTP Basic Auth 사용
- LAN 또는 사설망 사용 권장
- RCON은 폐기 예정

문서 버전 주의:

- 개별 주요 엔드포인트 페이지는 확인 시점에 `1.0.0`을 표시했다.
- REST 카테고리 색인은 확인 시점에 다른 버전 표기가 노출될 수 있었다.
- 따라서 코드 주석에는 사이트 전체 버전 대신 확인한 개별 URL과 날짜를 기록한다.

## 4. 서버 모드 기준

공식 서버 가이드에서 확인한 사항:

- 서버 측 모드는 Windows 전용 서버 기준
- 서버용 모드만 동작
- 모드는 저장 데이터 손상이나 충돌을 일으킬 수 있음
- 기본 Workshop 배치 경로는 서버 실행 파일 주변의 `Mods/Workshop`
- 활성화 식별자는 폴더명이 아니라 `Info.json`의 `PackageName`
- `Mods/PalModSettings.ini`에서 전역 활성화와 `ActiveModList` 사용
- 서버용 설치 규칙에는 `IsServer: true` 확인 필요
- 적용·업데이트·제거 뒤 서버 재시작 필요
- `-NoMods`는 모드를 강제로 비활성화

## 5. CHZZK Session 기준

확인한 연결 흐름:

```text
Access Token으로 사용자 세션 URL 요청
→ Socket.IO 연결
→ SYSTEM connected 수신
→ sessionKey 추출
→ DONATION 구독 요청
→ SYSTEM subscribed 확인
→ DONATION 이벤트 처리
```

확인한 운영 조건:

- 사용자 세션은 사용자별 최대 연결 제한이 있음
- 세션당 이벤트 구독 개수 제한이 있음
- Socket.IO-client 지원 범위가 공식 문서에 명시됨
- `revoked`는 권한 회수 상태

확인한 후원 필드:

```ts
interface ChzzkDonationPayload {
  donationType: 'CHAT' | 'VIDEO';
  channelId: string;
  donatorChannelId: string;
  donatorNickname: string;
  payAmount: string;
  donationText: string;
  emojis: Record<string, string>;
}
```

제한:

- 공식 후원 페이로드에 문서화된 고유 후원 이벤트 ID가 없다고 전제
- `payAmount`는 문자열
- 닉네임은 식별자 아님
- 후원 메시지는 신뢰할 수 없는 사용자 입력

## 6. CHZZK 인증 기준

- Authorization Code 흐름에서 `state` 검증 필요
- Access Token 갱신 시 Refresh Token 사용
- Refresh Token은 일회용으로 안내됨
- 갱신 응답의 새 Refresh Token을 원자적으로 저장해야 함
- 로그와 오류 응답에 토큰을 노출하지 않음

## 7. 아직 검증되지 않은 항목

다음은 실제 소스 코드와 테스트 서버가 준비되기 전까지 `unverified`이다.

- Palworld `v1.0.1`용 실제 `give_item` 함수 또는 클래스 경로
- Palworld `v1.0.1`용 실제 `spawn_pal` 함수 또는 클래스 경로
- 모든 아이템 내부 ID 카탈로그
- 모든 팰 내부 ID 카탈로그
- PalSchema와 Palworld `v1.0.1`의 전체 호환성
- Palworld용 UE4SS 포크의 `v1.0.1` 전체 안정성
- 서버 재시작 뒤 GameBridge 명령 멱등성
- 대규모 동시 후원이 실제 서버 성능에 미치는 영향

이 항목을 인터페이스나 모의 구현으로 분리하고 완료로 보고하지 않는다.

## 8. 다음 검증 절차

실제 서버 접근이 가능해지면:

1. 서버 실행 파일과 `/info`로 정확한 버전 기록
2. 월드 저장 폴더 전체 백업과 해시 생성
3. 모드 없이 REST 읽기 API 확인
4. `/save`와 정상 종료·재기동 확인
5. 사용하려는 모드 프레임워크 버전·해시 기록
6. 복사한 월드에서 모드 로드 확인
7. 읽기 전용 GameBridge 상태 확인
8. 허용 목록의 단일 아이템 지급 확인
9. 허용 목록의 단일 팰 소환 확인
10. 저장·종료·재기동 뒤 상태 확인
11. 실패·중복·타임아웃·롤백 확인
12. 검증된 ID만 버전 고정 데이터에 반영
