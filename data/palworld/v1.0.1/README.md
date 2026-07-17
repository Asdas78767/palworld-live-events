# Palworld v1.0.1 Data Snapshot

이 디렉터리는 `Palworld v1.0.1`에서 직접 검증된 내부 식별자와 메타데이터만 저장한다.

## 허용 파일

- `items.json`: 검증된 아이템 내부 ID
- `pals.json`: 검증된 팰 내부 ID
- `effects.json`: 검증된 효과 내부 ID
- `metadata.json`: 추출 출처와 도구 정보
- `verification.json`: 실제 테스트 서버 검증 기록

현재는 실제 대상 빌드 에셋 또는 테스트 서버가 제공되지 않았으므로 카탈로그 파일을 만들지 않는다. 빈 목록을 검증된 목록처럼 커밋하지 않는다.

## 필수 항목

각 레코드는 최소 다음 값을 가진다.

```json
{
  "id": "내부 식별자",
  "displayName": "표시 이름",
  "gameVersion": "1.0.1",
  "source": {
    "type": "asset-extraction 또는 runtime-enumeration",
    "path": "원본 경로 또는 함수",
    "sha256": "원본 해시"
  },
  "verified": {
    "status": "verified 또는 rejected",
    "serverVersion": "실제 /info 버전",
    "testedAt": "RFC3339 timestamp",
    "result": "검증 결과"
  }
}
```

## 금지

- 위키 표시 이름을 내부 ID로 추정
- 이전 게임 버전 목록을 버전만 바꿔 복사
- 출처 없는 gist 또는 블로그 목록 복사
- 실제 테스트 없이 `verified` 표시
- 실패한 ID를 허용 목록에 유지
