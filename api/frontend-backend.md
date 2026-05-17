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

## Open Questions

- 현재 발표 시연에서 필요한 API 목록은 무엇인가?
- 프론트엔드가 직접 처리해야 하는 에러 코드는 무엇인가?
- 인증이 없는 시연 모드를 둘 것인가?
