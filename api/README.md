# API Documentation Guide

Status: draft
Updated: 2026-05-19

## Purpose

이 폴더는 FreshKitchen 팀이 맞춰야 하는 API 연결 계약을 정리합니다.

API 문서는 백엔드 전체 명세가 아니라 파트 간 연결에 필요한 DTO, 흐름, 예시를 다룹니다.

## Documents

- `frontend-backend.md`: 프론트엔드와 백엔드 연결 계약
- `backend-ai.md`: 백엔드와 AI 서버 연결 계약
- `examples/`: 요청과 응답 예시 JSON

## Example Rules

예시 JSON은 연결 흐름을 검증하기 위한 문서용 파일입니다.

- 한 JSON 파일 안에 `endpoint`, `request`, `response`를 함께 씁니다.
- request-only 또는 response-only 파일을 새로 만들지 않습니다.
- 모든 API를 빠짐없이 나열하지 않고, 실제 연결에 쓰이는 흐름만 작성합니다.
- 긴 예시는 본문에 넣지 않고 `api/examples/` 아래에 둡니다.
- 예시는 JSON 문법이 통과해야 합니다.

기본 형태는 아래와 같습니다.

```json
{
  "domain": "item",
  "flow": "create",
  "endpoint": {
    "method": "POST",
    "path": "/api/v1/items"
  },
  "request": {
    "headers": {},
    "body": {}
  },
  "response": {
    "status": 201,
    "code": "COMMON-201",
    "message": "Success",
    "data": {}
  },
  "notes": []
}
```

## Frontend-Backend Examples

프론트엔드-백엔드 예시는 도메인 폴더 아래 작업 단위로 나눕니다.

```text
api/examples/frontend-backend/
  item/
    create.json
    update.json
    list.json
    detail.json
    delete.json
    consume.json
  analytics/
    summary.json
    expiring-items.json
  tips/
    storage.json
    recycling.json
  inquiry/
    create.json
  legal/
    info.json
    agreement-get.json
    agreement-post.json
  app/
    version.json
  scan/
    ingredient-image.json
    receipt-image.json
  home/
    summary.json
  common/
    error.json
```

Item처럼 CRUD 흐름이 있는 도메인은 작업 단위 파일로 구분합니다.

프론트엔드 request DTO는 내부 DB ID나 master data ID를 노출하지 않는 것을 기본으로 합니다.

- Item 생성/수정 request에는 `catalogId`를 넣지 않습니다.
- Item 생성/수정 request는 목표 계약 기준으로 `storageType`을 사용합니다.
- Item response에는 UI 표시용 `catalogId`, `category`, `emoji`를 포함할 수 있습니다.
- Storage type 공개 값은 `FRIDGE`, `FREEZER`, `PANTRY`입니다.

## Backend-AI Examples

백엔드-AI 예시는 현재 루트 파일 구조를 유지합니다.

- `examples/backend-ai-food-classification.json`
- `examples/backend-ai-receipt-ocr.json`
- `examples/backend-ai-fridge-detection.json`

## Validation

JSON 문법은 아래 명령으로 확인합니다.

```sh
find api/examples -name '*.json' -print0 | xargs -0 -n1 python3 -m json.tool >/dev/null
```
