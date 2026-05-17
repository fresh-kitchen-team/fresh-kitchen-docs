# Backend-AI API

Status: draft
Updated: 2026-05-17

## Purpose

이 문서는 FreshKitchen 백엔드와 AI 서버 사이의 API 계약을 정리합니다.

백엔드 팀원은 어떤 시점에 AI 서버를 호출하는지 확인하고, AI 팀원은 요청과 응답 형식을 맞춥니다.

## Current State

아직 실제 AI 서버 API가 모두 반영되지 않았습니다.

AI 서버 호출 방식이 확정되는 대로 이 문서를 갱신합니다.

## Call Direction

백엔드가 AI 서버에 요청을 보냅니다.

AI 서버는 백엔드 요청에 대한 분석 결과나 추천 결과를 응답합니다.

현재 백엔드 코드 기준으로 AI 서버 호출은 multipart file 요청을 사용합니다.

주요 경로는 아래와 같습니다.

- `/internal/v1/food-classification`
- `/internal/v1/receipt-ocr`
- `/internal/v1/fridge-detection`

## Failure Handling

백엔드는 AI 서버 호출 실패를 사용자 요청 전체 실패로 볼지, 대체 응답으로 처리할지 결정해야 합니다.

확인해야 할 실패 상황은 아래와 같습니다.

- AI 서버 연결 실패
- AI 서버 timeout
- AI 서버 4xx 응답
- AI 서버 5xx 응답
- 응답 형식 오류

## Endpoint Examples

엔드포인트별 예시는 본문에 길게 쓰지 않습니다.

하나의 엔드포인트는 하나의 JSON 파일로 관리합니다.

각 파일 안에서 `request`와 `response`를 구분합니다.

아래 파일을 갱신합니다.

- [backend-ai-food-classification.json](examples/backend-ai-food-classification.json)
- [backend-ai-receipt-ocr.json](examples/backend-ai-receipt-ocr.json)
- [backend-ai-fridge-detection.json](examples/backend-ai-fridge-detection.json)

현재 예시는 multipart 요청을 JSON으로 설명한 문서용 예시입니다.

## Open Questions

- AI 서버 timeout 기준은 몇 초인가?
- AI 서버 실패 시 백엔드는 어떤 에러 코드로 응답하는가?
- 발표 시연에서 AI 서버를 실시간 호출할 것인가?
