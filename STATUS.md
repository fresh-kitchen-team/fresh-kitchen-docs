# Status

Status: draft
Updated: 2026-05-19

## Summary

FreshKitchen 문서 저장소의 초기 구조를 정리하고, API 연결 계약을 파트별로 문서화하는 단계입니다.

## Done

- 문서 저장소의 목적을 정했습니다.
- 루트 문서 중심으로 운영하기로 정했습니다.
- API 문서는 프론트엔드-백엔드, 백엔드-AI 경계로 나누기로 정했습니다.
- 문서 작업용 AI 규칙과 커밋 skill을 두기로 정했습니다.
- API 예시 파일의 기본 위치와 구조를 만들었습니다.
- Analytics API (summary, expiring-items) 구현 및 문서화를 완료했습니다.
- Tips API (storage, recycling) 구현 및 문서화를 완료했습니다.
- Item Consume API 구현 및 문서화를 완료했습니다.
- Store Release API (inquiry, app version, legal) 구현 및 문서화를 완료했습니다.
- API 명세서를 Notion에 작성했습니다.

## Doing

- 루트 문서의 초안을 작성합니다.
- 발표와 시연 준비 문서를 한 파일에서 확인할 수 있게 정리합니다.

## Blocked

- 실제 API 요청/응답 값은 백엔드 구현과 맞춰 갱신해야 합니다.
- 발표 일정과 담당자는 팀 합의 후 확정해야 합니다.

## Progress Scope

이 저장소는 현재 진행 상태를 `STATUS.md`에서 관리합니다.

루트 `progress/` 폴더는 현재 만들지 않습니다.

12~16주차 예정 ToDo는 `ROADMAP.md`에서 관리합니다.

`STATUS.md`에는 주차별 상세 이력을 누적하지 않고, 현재 기준의 완료, 진행, 막힘, 다음 작업만 남깁니다.

포함하는 내용은 아래와 같습니다.

- 지금까지 완료된 주요 작업
- 현재 진행 중인 작업
- 막힌 작업
- 다음에 확인하거나 진행할 작업

포함하지 않는 내용은 아래와 같습니다.

- 주차별 회고 누적
- 오래된 완료 이력 보관
- 개인별 상세 작업일지
- 회의록 대체 문서

수업, 제출, 평가 기준에서 주차별 산출물 파일을 명시적으로 요구하면 `progress/` 폴더를 다시 검토합니다.

그 전까지는 오래된 진행상황을 보관하지 않고, 현재 기준으로 이 파일을 갱신합니다.

## Next

- `SCOPE.md`에 이번 발표 또는 제출 기준 기능 범위를 채웁니다.
- `api/frontend-backend.md`에 현재 구현된 API를 반영합니다.
- `api/backend-ai.md`에 AI 서버 호출 계약을 반영합니다.
- `PRESENTATION.md`에 실제 시연 순서를 확정합니다.
