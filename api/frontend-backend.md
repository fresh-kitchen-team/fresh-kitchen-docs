# Frontend-Backend API

Status: draft
Updated: 2026-05-17

## Purpose

이 문서는 FreshKitchen 프론트엔드와 백엔드 사이의 API 계약을 정리합니다.

프론트엔드 팀원은 이 문서를 보고 어떤 요청을 보내야 하는지 확인하고, 백엔드 팀원은 응답 형식과 실패 처리 기준을 맞춥니다.

## Current State

아직 실제 구현 API가 모두 반영되지 않았습니다.

백엔드 구현이 확정되는 대로 이 문서를 갱신합니다.

## Common Response

백엔드는 성공 응답을 공통 응답 형식으로 내려줍니다.

현재 백엔드의 성공 응답은 `status`, `code`, `message`, `data`를 포함합니다.

예시는 아래 파일에서 확인합니다.

- [frontend-backend-response.json](examples/frontend-backend-response.json)

## Error Response

에러 응답은 클라이언트가 `message`보다 `code`를 기준으로 처리하는 것을 기본으로 합니다.

에러 응답에는 보통 아래 정보가 포함됩니다.

- 발생 시각
- HTTP 상태 코드
- 서비스 내부 에러 코드
- 사용자 또는 클라이언트가 읽을 메시지
- 요청 경로

## Authentication

인증 방식은 백엔드 구현 기준에 맞춰 갱신합니다.

확정되기 전까지는 아래 내용을 확인해야 합니다.

- access token 전달 위치
- refresh token 사용 여부
- 인증이 필요한 API 목록
- 인증 실패 시 응답 코드

## Request Examples

요청 예시는 본문에 길게 쓰지 않습니다.

아래 파일을 갱신합니다.

- [frontend-backend-request.json](examples/frontend-backend-request.json)

현재 예시는 식재료 생성 요청을 기준으로 작성되어 있습니다.

## Functional Flow Validation

`api/examples/`의 예시는 JSON 문법만 맞는지보다 실제 기능 흐름에서 DTO와 API가 맞물리는지 확인하기 위해 사용합니다.

예시는 아래 질문에 답할 수 있어야 합니다.

- 프론트엔드가 요청에 필요한 값을 모두 보낼 수 있는가?
- 백엔드는 요청 DTO를 받아 저장, 조회, AI 호출 같은 다음 단계로 넘길 수 있는가?
- 백엔드 응답 DTO는 프론트엔드가 화면을 갱신하는 데 필요한 값을 포함하는가?
- 프론트엔드가 표시하는 값과 백엔드가 저장하는 값이 구분되는가?
- 실패가 발생하면 어느 구간에서 실패했는지 설명할 수 있는가?

기능 흐름은 아래처럼 확인합니다.

직접 식재료 등록:

`프론트엔드 입력 폼 -> 백엔드 식재료 생성 API -> 백엔드 저장 -> 저장된 식재료 ID 응답 -> 프론트엔드 목록 또는 상세 화면 갱신`

이미지 기반 식재료 등록:

`프론트엔드 이미지 업로드 -> 백엔드 API -> AI classification 호출 -> 백엔드 category 매핑 -> 식재료 후보 또는 저장 결과 응답 -> 프론트엔드 확인 화면 표시`

영수증 OCR 등록:

`프론트엔드 영수증 이미지 업로드 -> 백엔드 API -> AI OCR 호출 -> 품목별 category 포함 결과 수신 -> 백엔드 저장 또는 확인 응답 -> 프론트엔드 품목 목록 표시`

예시 파일을 갱신할 때는 해당 JSON이 위 흐름 중 어느 단계의 요청 또는 응답인지 설명할 수 있어야 합니다.

JSON 문법은 기본 확인으로만 아래 명령을 사용합니다.

```sh
find api/examples -name '*.json' -print0 | xargs -0 -n1 python3 -m json.tool >/dev/null
```

## Open Questions

- 현재 발표 시연에서 필요한 API 목록은 무엇인가?
- 프론트엔드가 직접 처리해야 하는 에러 코드는 무엇인가?
- 인증이 없는 시연 모드를 둘 것인가?
