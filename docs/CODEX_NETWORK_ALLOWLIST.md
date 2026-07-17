# Codex Network Allowlist

이 문서는 Codex Cloud에서 인터넷 접근을 허용해야 할 때의 최소 도메인 집합과 사용 목적을 정의한다.

## 1. 기본 정책

- 작업 단계 인터넷 접근은 기본 차단을 우선한다.
- 문서 확인에는 `GET`과 `HEAD`만 허용한다.
- 실제 CHZZK 토큰 발급, 실제 후원 구독, 실제 Palworld 서버 제어는 Codex Cloud에서 수행하지 않는다.
- 비밀키나 운영 서버 주소를 Codex 환경 변수에 넣지 않는다.
- 새 도메인은 사용 목적과 신뢰 등급을 문서화한 뒤 추가한다.

## 2. 공식 문서 도메인

| 도메인 | 목적 | 권장 메서드 |
|---|---|---|
| `docs.palworldgame.com` | Palworld 서버·REST·모드 공식 문서 | GET, HEAD |
| `www.pocketpair.jp` | Pocketpair 공식 소식 | GET, HEAD |
| `steamcommunity.com` | Palworld 공식 공지와 Workshop 메타데이터 | GET, HEAD |
| `store.steampowered.com` | Palworld 공식 패치 뉴스 | GET, HEAD |
| `chzzk.gitbook.io` | CHZZK 공식 API 문서 | GET, HEAD |
| `developers.openai.com` | Codex 공식 문서 | GET, HEAD |
| `nodejs.org` | Node.js 공식 문서 | GET, HEAD |
| `www.typescriptlang.org` | TypeScript 공식 문서 | GET, HEAD |
| `pnpm.io` | pnpm 공식 문서 | GET, HEAD |
| `socket.io` | Socket.IO 공식 문서 | GET, HEAD |
| `zod.dev` | Zod 공식 문서 | GET, HEAD |
| `fastify.dev` | Fastify 공식 문서 | GET, HEAD |
| `vitest.dev` | Vitest 공식 문서 | GET, HEAD |
| `docs.docker.com` | Docker 공식 문서 | GET, HEAD |

## 3. 저장소와 패키지 메타데이터

| 도메인 | 목적 | 권장 메서드 |
|---|---|---|
| `github.com` | 공식 저장소·릴리스·이슈 확인 | GET, HEAD |
| `api.github.com` | 공개 저장소 메타데이터 확인 | GET, HEAD |
| `raw.githubusercontent.com` | 공식 저장소의 특정 텍스트 파일 확인 | GET, HEAD |
| `registry.npmjs.org` | 패키지 버전·무결성 메타데이터 | GET, HEAD |

패키지 설치가 필요한 설정 단계에서만 패키지 레지스트리 접근을 허용한다. 에이전트 작업 중 임의 최신 버전 설치를 허용하지 않는다.

## 4. 보조 커뮤니티 자료

| 도메인 | 목적 | 신뢰 등급 |
|---|---|---|
| `pwmodding.wiki` | Palworld 모딩 보조 문서 | B |
| `palworld.wiki.gg` | 표시 이름·패치 이력 탐색 | B |
| `www.nexusmods.com` | 특정 도구의 배포 메타데이터 | B |

보조 자료는 공식 자료와 실제 테스트로 재검증한다.

## 5. 기본적으로 허용하지 않는 런타임 도메인

다음 도메인은 애플리케이션 런타임에는 필요할 수 있지만 Codex 문서 조사용 allowlist에는 넣지 않는다.

- `openapi.chzzk.naver.com`
- `chzzk.naver.com`
- 운영 Palworld 서버 호스트
- 운영 GameBridge 호스트
- 운영 데이터베이스 호스트

실제 통합 테스트가 필요하면 사용자 승인, 격리된 테스트 계정, 최소 권한, 별도 테스트 서버를 사용한다.
