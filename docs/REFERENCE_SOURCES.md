# Reference Sources

최종 확인일: `2026-07-17`

이 문서는 Codex가 치지직 후원과 Palworld 전용 서버 연동을 구현할 때 사용할 자료의 우선순위, 용도, 제한을 정의한다.

## 1. 사용 원칙

- 공식 명세가 존재하면 공식 명세를 최우선으로 사용한다.
- 검색 결과 요약만으로 API를 구현하지 않고 원문 페이지를 확인한다.
- 페이지 제목이나 사이트 전체 버전 표시만 보지 않고 정확한 엔드포인트 페이지, 요청·응답 스키마, 변경 이력을 확인한다.
- 패치·API·모드 프레임워크처럼 변경 가능성이 큰 정보는 관련 작업마다 다시 확인한다.
- 커뮤니티 자료는 탐색과 교차 검증에만 사용하며 공식 명세를 대체하지 않는다.
- 내부 아이템·팰 ID는 커뮤니티 표만으로 프로덕션 허용 목록에 넣지 않는다.
- 외부 페이지의 코드를 그대로 복사하지 않고 현재 프로젝트의 타입, 보안, 오류 처리 기준에 맞게 재구성한다.

## 2. 신뢰 등급

| 등급 | 의미 | 구현 사용 범위 |
|---|---|---|
| `A0` | 개발사 또는 서비스 운영사의 공식 명세·공지 | API 계약과 패치 기준으로 직접 사용 가능 |
| `A1` | 사용 중인 도구 유지관리자의 공식 저장소·릴리스 | 해당 도구의 기능과 호환성 판단에 사용 가능 |
| `B` | 관리되는 전문 커뮤니티 문서·위키 | 후보 탐색과 보조 설명에 사용, 재검증 필수 |
| `C` | 포럼·댓글·영상·개인 블로그·검색 결과 | 문제 탐색에만 사용, 구현 근거로 단독 사용 금지 |

## 3. Palworld 공식 자료 — A0

### 3.1 게임 공지와 패치

- Pocketpair 게임 소식
  - `https://www.pocketpair.jp/game-news/`
  - 용도: 정식 출시, 대형 업데이트, 개발사 공지 확인
- Pocketpair Palworld 소식 목록
  - `https://www.pocketpair.jp/game-news/category_game/palworld-ja/`
  - 용도: Palworld 관련 공식 소식 필터링
- Steam Palworld 공식 커뮤니티 허브
  - `https://steamcommunity.com/app/1623730`
  - 용도: 공식 패치 공지와 Steam 빌드 관련 공지 확인
- Steam Palworld 뉴스
  - `https://store.steampowered.com/news/app/1623730`
  - 용도: 공식 패치 노트 원문 확인

패치 정보 사용 규칙:

1. 개발사 공지 또는 Steam 공식 공지에서 버전과 날짜를 확인한다.
2. 플랫폼별 차이가 있으면 대상 플랫폼을 명시한다.
3. 패치 노트에 없는 동작 변경을 추정하지 않는다.
4. 모드 호환성은 패치 번호만으로 판단하지 않고 실제 로드 테스트를 수행한다.

### 3.2 전용 서버 가이드

- Server Guide 루트
  - `https://docs.palworldgame.com/`
- 서버 설치
  - `https://docs.palworldgame.com/getting-started/deploy-dedicated-server/`
- 서버 설정
  - `https://docs.palworldgame.com/settings-and-operation/configuration/`
- 실행 인수
  - `https://docs.palworldgame.com/settings-and-operation/arguments/`
- 서버 성능
  - `https://docs.palworldgame.com/settings-and-operation/performance/`
- 서버 모드 설치
  - `https://docs.palworldgame.com/settings-and-operation/mod/`
- API 색인
  - `https://docs.palworldgame.com/category/api/`
- REST API 색인
  - `https://docs.palworldgame.com/category/rest-api/`
- RCON 문서
  - `https://docs.palworldgame.com/api/rcon/`

주의:

- 문서 사이트의 색인 페이지와 개별 페이지에 표시되는 버전이 다를 수 있다.
- 구현에서는 정확한 개별 페이지 URL과 실제 서버 `/info` 응답을 기록한다.
- RCON은 공식 문서에서 폐기 예정으로 안내되므로 신규 구현에 사용하지 않는다.
- 공식 서버 모드 문서 기준 서버 측 모드는 Windows 전용 서버를 기준으로 검증한다.

### 3.3 REST API 개별 명세

기본 경로는 서버 설정에 따른 호스트와 REST 포트의 `/v1/api`이다.

- 소개
  - `https://docs.palworldgame.com/api/rest-api/palwold-rest-api/`
- 서버 정보
  - `https://docs.palworldgame.com/api/rest-api/info/`
- 플레이어 목록
  - `https://docs.palworldgame.com/api/rest-api/players/`
- 서버 설정 조회
  - `https://docs.palworldgame.com/api/rest-api/settings/`
- 서버 지표
  - `https://docs.palworldgame.com/api/rest-api/metrics/`
- 공지
  - `https://docs.palworldgame.com/api/rest-api/announce/`
- 강퇴
  - `https://docs.palworldgame.com/api/rest-api/kick/`
- 밴
  - `https://docs.palworldgame.com/api/rest-api/ban/`
- 밴 해제
  - `https://docs.palworldgame.com/api/rest-api/unban/`
- 월드 저장
  - `https://docs.palworldgame.com/api/rest-api/save/`
- 정상 종료
  - `https://docs.palworldgame.com/api/rest-api/shutdown/`
- 강제 종료
  - `https://docs.palworldgame.com/api/rest-api/stop/`

REST 사용 규칙:

- `GET /settings`는 조회 전용으로 취급한다.
- 아이템 지급, 팰 소환, 체력 조작, 순간이동은 공식 REST 기능으로 가정하지 않는다.
- HTTP Basic Auth 자격 증명을 코드, URL, 로그에 남기지 않는다.
- REST 포트를 공개 인터넷에 직접 노출하지 않는다.
- 응답 스키마는 런타임 검증하고 문서와 실제 응답 차이를 기록한다.

## 4. CHZZK 공식 자료 — A0

### 4.1 기본과 인증

- Developers 루트
  - `https://chzzk.gitbook.io/chzzk`
- 인증
  - `https://chzzk.gitbook.io/chzzk/chzzk-api/authorization`
- 공통 참고사항
  - `https://chzzk.gitbook.io/chzzk/chzzk-api/tips`

인증 사용 규칙:

- OAuth `state`를 생성하고 콜백에서 검증한다.
- Refresh Token은 일회용 갱신 토큰으로 취급하며 갱신 결과의 새 토큰으로 교체한다.
- Open API 도메인과 계정 연동 도메인을 혼동하지 않는다.
- 실제 토큰을 Codex 작업, 테스트 fixture, 로그에 제공하지 않는다.

### 4.2 사용자·채널·방송·채팅

- 사용자
  - `https://chzzk.gitbook.io/chzzk/chzzk-api/user`
- 채널
  - `https://chzzk.gitbook.io/chzzk/chzzk-api/channel`
- 라이브
  - `https://chzzk.gitbook.io/chzzk/chzzk-api/live`
- 채팅
  - `https://chzzk.gitbook.io/chzzk/chzzk-api/chat`

식별 규칙:

- 닉네임이나 채널 이름을 고유 계정 ID로 사용하지 않는다.
- 계정 매핑에는 공식 `channelId`를 사용한다.
- 표시 문자열은 변경·중복·다국어 가능성을 전제로 한다.

### 4.3 세션과 후원

- 세션
  - `https://chzzk.gitbook.io/chzzk/chzzk-api/session`

현재 확인 기준 핵심 사항:

- 후원 수신은 `DONATION` 세션 이벤트 구독을 사용한다.
- 사용자 세션은 Access Token과 동일한 사용자의 이벤트를 구독한다.
- 공식 문서가 안내하는 Socket.IO-client 지원 범위를 벗어나지 않는다.
- `SYSTEM connected`의 `sessionKey`를 받은 뒤 구독한다.
- `subscribed` 확인 전에는 준비 완료로 처리하지 않는다.
- `revoked`는 권한 철회 상태로 처리한다.
- 후원 금액 `payAmount`는 문자열이다.
- 공식 후원 메시지 스키마에 없는 고유 후원 ID를 임의 생성해 공식 필드처럼 취급하지 않는다.

### 4.4 Drops와 Webhook

- Drops API
  - `https://chzzk.gitbook.io/chzzk/chzzk-api/drops`
- Webhook Event
  - `https://chzzk.gitbook.io/chzzk/drops/webhook`

Drops 문서는 일반 후원 이벤트와 별개이다. 후원 수신을 Drops webhook으로 구현하지 않는다.

Webhook을 사용하는 기능에서는:

- HTTPS와 서명 검증을 적용한다.
- 원문 body를 기준으로 HMAC을 검증한다.
- 메시지 ID 기준 중복 처리를 구현한다.
- 재전송 정책을 고려한다.

## 5. Palworld 모딩 도구 — A1

### 5.1 PalSchema

- 저장소
  - `https://github.com/Okaetsu/PalSchema`
- 릴리스
  - `https://github.com/Okaetsu/PalSchema/releases`
- Nexus 배포 페이지
  - `https://www.nexusmods.com/palworld/mods/2361`

사용 규칙:

- 릴리스 태그, 커밋 SHA, 알려진 문제를 기록한다.
- Nexus의 버전 표시는 배포 확인용이며 기능 명세는 GitHub 릴리스와 저장소를 우선한다.
- 대상 Palworld 빌드와 실제 전용 서버에서 로드 검증을 수행한다.

### 5.2 UE4SS

- Palworld용 유지관리 포크
  - `https://github.com/Okaetsu/RE-UE4SS`
- Palworld 실험 릴리스
  - `https://github.com/Okaetsu/RE-UE4SS/releases/tag/experimental-palworld`
- 업스트림 UE4SS
  - `https://github.com/UE4SS-RE/RE-UE4SS`
- 업스트림 문서
  - `https://docs.ue4ss.com/`

사용 규칙:

- 업스트림과 Palworld 전용 포크를 혼용하지 않는다.
- 바이너리 파일명만으로 동일 버전이라고 판단하지 않는다.
- 태그, 커밋, 파일 해시를 기록한다.
- 게임 업데이트 후 이전 시그니처나 오프셋을 재사용하지 않는다.

## 6. 관리되는 커뮤니티 자료 — B

### 6.1 Palworld Modding Docs

- `https://pwmodding.wiki/`
- Workshop 소개
  - `https://pwmodding.wiki/docs/users/workshop/introduction`

용도:

- 모드 제작·패키징 흐름 이해
- `Info.json`, Workshop, PalSchema, UE4SS 개념 탐색
- 일반적인 문제 해결 후보 수집

제한:

- Pocketpair 공식 서버 가이드와 충돌하면 공식 가이드를 우선한다.
- 문서의 예제 버전이 대상 `v1.0.1`과 다르면 그대로 사용하지 않는다.

### 6.2 Palworld Wiki

- `https://palworld.wiki.gg/`
- 아이템 목록
  - `https://palworld.wiki.gg/wiki/Items`
- 버전 1.0.1
  - `https://palworld.wiki.gg/wiki/1.0.1`

용도:

- 표시 이름, 설명, 패치 이력, 인간이 읽기 쉬운 분류 탐색

제한:

- 번역명과 내부 데이터 ID를 동일시하지 않는다.
- 위키 정보만으로 지급·소환 허용 목록을 만들지 않는다.
- 공식 패치 공지가 존재하면 공식 공지를 우선한다.

### 6.3 Steam Workshop

- `https://steamcommunity.com/app/1623730/workshop/`

용도:

- 특정 모드의 Workshop ID, 게시자, 업데이트 시각, 의존성 후보 확인

제한:

- 설명문만으로 서버 호환성을 확정하지 않는다.
- 실제 패키지의 `Info.json`, `PackageName`, `InstallRule`, `IsServer`, 의존성을 확인한다.
- 사용자 댓글은 오류 재현 후보로만 사용한다.

## 7. 언어와 런타임 공식 자료 — A0/A1

실제 저장소에서 사용하는 도구만 참조한다. 새 도구를 추가하기 위한 명분으로 사용하지 않는다.

- Node.js
  - `https://nodejs.org/docs/latest/api/`
- TypeScript
  - `https://www.typescriptlang.org/docs/`
- pnpm
  - `https://pnpm.io/`
- npm Registry
  - `https://registry.npmjs.org/`
- Socket.IO
  - `https://socket.io/docs/`
- Zod
  - `https://zod.dev/`
- Fastify
  - `https://fastify.dev/docs/latest/`
- Vitest
  - `https://vitest.dev/guide/`
- PostgreSQL
  - `https://www.postgresql.org/docs/`
- SQLite
  - `https://www.sqlite.org/docs.html`
- Docker
  - `https://docs.docker.com/`

의존성 검증 규칙:

- 현재 잠금 파일에 고정된 버전을 먼저 확인한다.
- 공식 문서의 최신 버전 예제를 현재 버전에 무조건 적용하지 않는다.
- 메이저 버전 차이가 있으면 해당 버전 문서를 찾는다.
- 보안 패치가 필요한 경우 변경 범위와 마이그레이션 위험을 보고한다.

## 8. Codex 공식 자료 — A0

- Codex 문서 루트
  - `https://developers.openai.com/codex/`
- AGENTS.md 안내
  - `https://developers.openai.com/codex/guides/agents-md`
- Codex 모범 사례
  - `https://developers.openai.com/codex/learn/best-practices`
- Cloud 환경
  - `https://developers.openai.com/codex/cloud/environments`

Codex 관련 설정이 실제 UI 또는 문서와 다르면 공식 현재 문서를 다시 확인한다. 저장소 지침은 루트와 하위 디렉터리의 `AGENTS.md`로 관리한다.

## 9. 내부 ID 데이터의 출처 규칙

### 9.1 허용되는 최종 출처

프로덕션 `itemId`, `palId`, 클래스 경로는 다음 중 하나에서 얻어야 한다.

- 대상 빌드의 데이터 테이블 또는 에셋에서 추출한 값
- 대상 빌드에서 검증된 모드 프레임워크의 런타임 열거 결과
- 프로젝트가 직접 생성하고 테스트한 버전 고정 카탈로그

### 9.2 허용되지 않는 최종 출처

- 번역된 표시 이름
- 위키 표의 이름만 복사한 값
- 오래된 블로그의 치트 명령 목록
- 출처와 버전이 없는 GitHub gist
- 영상 자막이나 댓글
- AI가 추정한 클래스명 또는 ID

### 9.3 필수 메타데이터

각 데이터 파일은 최소 다음을 기록한다.

```json
{
  "schemaVersion": 1,
  "gameVersion": "1.0.1",
  "platform": "steam-windows-dedicated-server",
  "sourceType": "asset-extraction",
  "sourcePath": "원본 데이터 경로 또는 런타임 열거 지점",
  "extractedAt": "RFC3339 timestamp",
  "extractor": {
    "name": "도구 이름",
    "version": "버전 또는 commit SHA"
  },
  "sourceSha256": "원본 또는 산출물 SHA-256",
  "verifiedOnServerAt": null,
  "verifiedServerVersion": null
}
```

## 10. 충돌 해결

정보가 충돌하면 다음 순서로 판단한다.

1. 대상 서버가 반환한 실제 버전과 응답
2. 현재 개별 공식 명세 페이지
3. 현재 공식 패치 공지
4. 도구 유지관리자의 대상 버전 릴리스
5. 저장소의 검증된 스냅샷
6. 커뮤니티 자료

결론을 내리지 못하면 기능을 비활성화하고 `unverified`로 보고한다.

## 11. 자료 갱신 절차

자료를 갱신할 때:

1. 공식 패치 공지 확인
2. Palworld Server Guide의 관련 개별 페이지 확인
3. CHZZK 관련 개별 페이지 확인
4. 사용 중인 모드 프레임워크 릴리스 확인
5. 실제 서버 버전과 기능 확인
6. `docs/VERIFIED_BASELINE.md` 갱신
7. 내부 데이터 스냅샷의 버전·해시 갱신
8. 테스트 결과 기록

단순히 문서 날짜만 바꾸지 않는다. 실제로 확인한 항목과 확인하지 못한 항목을 분리한다.
