# Roadmap

Status: draft
Updated: 2026-05-17

## Purpose

이 문서는 FreshKitchen 문서와 프로젝트 준비의 남은 작업 순서를 정리합니다.

정확한 날짜보다 지금 무엇을 먼저 맞춰야 하는지에 집중합니다.

## v0.1 Docs Foundation

문서 저장소를 팀원이 읽을 수 있는 상태로 만듭니다.

- 루트 문서 구조를 확정합니다.
- API 문서 위치를 나눕니다.
- 문서 작업 규칙을 `AGENTS.md`에 적습니다.
- 문서 저장소용 커밋 skill을 준비합니다.

## v0.2 Scope Alignment

발표와 시연에 필요한 프로젝트 범위를 맞춥니다.

- `SCOPE.md`에 포함 기능과 제외 기능을 확정합니다.
- `STATUS.md`에 현재 진행 상황을 반영합니다.
- 프론트엔드, 백엔드, AI 파트의 남은 작업을 확인합니다.

## v0.3 API Alignment

파트 간 API 계약을 맞춥니다.

- 프론트엔드-백엔드 API 계약을 작성합니다.
- 백엔드-AI API 계약을 작성합니다.
- 요청과 응답 예시를 `api/examples/`에 분리합니다.
- 에러 응답과 실패 처리 기준을 정리합니다.

12주차에는 AI-백엔드 DTO 추가사항을 진행합니다.

- classification 응답의 `source` 값과 category 매핑 기준을 맞춥니다.
- OCR 응답의 품목별 category DTO를 백엔드 저장 흐름에 맞춥니다.
- AI 서버 원본 응답과 백엔드 보강 DTO의 차이를 문서와 예시에 반영합니다.

프론트엔드-백엔드 Item DTO는 내부 DB ID가 프론트 계약에 새지 않도록 정리합니다.

- Item 생성/수정 request DTO에서 `catalogId`를 제거합니다.
- 백엔드는 `name` 또는 AI/OCR 인식 결과명을 catalog master data에 매핑합니다.
- Item 조회/list/detail response DTO에는 `catalogId`, `category`, `emoji`를 포함해 UI가 카테고리를 표시할 수 있게 합니다.
- Item 생성/수정 request DTO에서 `storageId`를 `storageType`으로 전환합니다.
- 백엔드는 인증 사용자와 `storageType`으로 유저별 storage ID를 찾아 매핑합니다.
- Storage type 공개 값은 `FRIDGE`, `FREEZER`, `PANTRY`로 맞춥니다.
- API 예시는 도메인 폴더와 흐름별 JSON으로 관리하고, 한 파일 안에 `endpoint`, `request`, `response`를 함께 작성합니다.

## v0.4 Presentation Ready

발표와 시연을 바로 연습할 수 있는 상태로 만듭니다.

- 발표 흐름을 확정합니다.
- 시연 순서를 확정합니다.
- 담당자를 정합니다.
- 실패 시 대체 흐름을 준비합니다.

## Update Rule

로드맵은 완료 이력을 길게 보관하지 않습니다.

단계가 끝나면 현재 기준으로 내용을 갱신하고, 다음에 확인해야 할 작업만 남깁니다.

진행 상태는 `STATUS.md`에서 관리합니다.

`ROADMAP.md`는 앞으로 맞출 단계와 순서에 집중하고, `STATUS.md`는 지금 완료, 진행, 막힘, 다음 작업에 집중합니다.
