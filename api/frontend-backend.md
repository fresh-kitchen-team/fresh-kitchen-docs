# Frontend-Backend API

Status: draft
Updated: 2026-05-19

## Purpose

이 문서는 FreshKitchen 프론트엔드와 백엔드 사이의 API 연결 계약을 정리합니다.

전체 백엔드 API 명세를 모두 적는 문서가 아니라, 프론트 화면과 백엔드 구현이 맞물리는 DTO와 흐름을 맞추기 위한 문서입니다.

예시 JSON 작성 규칙은 [api/README.md](README.md)를 따릅니다.

## Current State

이 문서는 `freshkitchen` 백엔드의 실제 구현과 앞으로 맞출 목표 계약을 함께 적습니다.

현재 구현 기준은 아래 코드에서 확인한 내용입니다.

- `ItemRequest.Create`: 생성 요청은 `catalogId`를 받지 않고 `storageId`를 받습니다.
- `ItemRequest.Update`: 현재 구현은 `catalogId`, `storageId`를 받을 수 있지만 목표 계약에서는 제거 또는 전환합니다.
- `ItemResponse.Item`: 조회 응답은 `catalogId`, `category`, `emoji`를 포함합니다.
- `ScanDto`: 영수증 스캔 응답에는 현재 `imageAsset`, `ocrText`, `confidence`가 없습니다.

목표 계약은 아래 방향으로 맞춥니다.

- 프론트 요청 DTO에서 `catalogId`를 제거합니다.
- 백엔드는 식재료명 또는 AI/OCR 인식 결과명을 catalog master data에 매핑합니다.
- 응답 DTO에는 `catalogId`, `category`, `emoji`를 포함해 UI가 카테고리와 표시값을 사용할 수 있게 합니다.
- 프론트 요청 DTO에서 유저별 DB ID인 `storageId`를 직접 보내지 않고 `storageType`을 보내도록 전환합니다.
- 백엔드는 `userId + storageType`으로 해당 유저의 storage를 찾아 내부 `storageId`에 매핑합니다.

## Example Files

프론트-백엔드 예시는 도메인 폴더와 흐름별 JSON 파일로 관리합니다.

각 JSON 파일은 `endpoint`, `request`, `response`를 함께 포함합니다.

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

## Common Response

백엔드는 성공 응답을 공통 응답 형식으로 내려줍니다.

```json
{
  "status": 200,
  "code": "COMMON-200",
  "message": "Success",
  "data": {}
}
```

생성 성공은 `201`, 일반 성공은 `200`을 사용합니다.

에러 응답은 `ApiResponse` wrapper를 사용하지 않고 `timestamp`, `status`, `code`, `message`, `path`를 포함합니다.

- [common/error.json](examples/frontend-backend/common/error.json)

## Authentication

API는 인증된 사용자 context를 기준으로 동작합니다.

프론트는 인증 확정 후 아래 형식으로 access token을 전달하는 것을 기본으로 맞춥니다.

```http
Authorization: Bearer {accessToken}
```

인증 방식과 토큰 갱신 정책은 User/Auth API가 확정되면 갱신합니다.

## Item API

Item API는 식재료 등록, 수정, 조회, 삭제 흐름에서 사용합니다.

| Flow | Method | Path | Example |
|---|---|---|---|
| create | `POST` | `/api/v1/items` | [item/create.json](examples/frontend-backend/item/create.json) |
| update | `PATCH` | `/api/v1/items/{itemId}` | [item/update.json](examples/frontend-backend/item/update.json) |
| list | `GET` | `/api/v1/items` | [item/list.json](examples/frontend-backend/item/list.json) |
| detail | `GET` | `/api/v1/items/{itemId}` | [item/detail.json](examples/frontend-backend/item/detail.json) |
| delete | `DELETE` | `/api/v1/items/{itemId}` | [item/delete.json](examples/frontend-backend/item/delete.json) |

### Catalog DTO Policy

Catalog는 백엔드 master data입니다.

프론트 요청 DTO에는 `catalogId`를 넣지 않습니다. 프론트는 사용자가 입력하거나 AI/OCR에서 확인한 식재료명을 보냅니다.

백엔드는 식재료명을 catalog master data에 매핑하고, 조회 응답에 UI 표시용 값을 포함합니다.

- `catalogId`: 백엔드 catalog master data ID
- `category`: UI 카테고리 표시값
- `emoji`: UI 기본 아이콘 표시값

프론트는 catalog API나 catalog ID 입력 없이 응답의 `category`, `emoji`를 그대로 표시합니다.

Catalog 매핑 실패 시 fallback 정책은 아직 구현 필요 항목입니다.

### Storage DTO Policy

현재 구현은 Item 생성/수정 요청에서 `storageId`를 받습니다.

목표 계약은 `storageId` 대신 `storageType`을 받는 것입니다. 유저마다 냉장고, 냉동고, 팬트리 storage row의 ID가 다르기 때문에 프론트가 DB ID를 관리하지 않도록 합니다.

허용값은 실제 백엔드 enum 기준입니다.

- `FRIDGE`
- `FREEZER`
- `PANTRY`

백엔드는 인증 사용자와 `storageType`으로 해당 유저의 storage를 찾고, 없으면 기본 storage를 보장한 뒤 내부 `storageId`로 매핑합니다.

## Scan API

Scan API는 multipart 이미지를 받아 AI 분석 결과를 프론트 확인 화면에 보여주기 위한 응답을 반환합니다.

| Flow | Method | Path | Example |
|---|---|---|---|
| ingredient image | `POST` | `/api/v1/scan/ingredient-image` | [scan/ingredient-image.json](examples/frontend-backend/scan/ingredient-image.json) |
| receipt image | `POST` | `/api/v1/scan/receipt-image` | [scan/receipt-image.json](examples/frontend-backend/scan/receipt-image.json) |

스캔 결과는 식재료를 바로 저장하지 않습니다. 프론트가 결과를 확인하거나 수정한 뒤 Item 생성 API로 최종 저장합니다.

식재료 이미지 스캔 응답의 `imageAsset.imageAssetId`는 Item 생성 요청의 `imageAssetId`로 연결할 수 있습니다.

## Home Summary API

홈 요약 API는 전체 개수, 신선도 상태별 개수, storage별 개수, 소비 임박/만료/최근 식재료 preview를 한 번에 반환합니다.

| Flow | Method | Path | Example |
|---|---|---|---|
| summary | `GET` | `/api/v1/home/summary` | [home/summary.json](examples/frontend-backend/home/summary.json) |

## Functional Flow Validation

`api/examples/`의 예시는 JSON 문법만 맞는지보다 실제 기능 흐름에서 DTO와 API가 맞물리는지 확인하기 위해 사용합니다.

직접 식재료 등록:

`프론트엔드 입력 폼 -> 백엔드 Item 생성 API -> catalog master data 매핑 -> storageType을 유저 storageId로 매핑 -> 저장된 item ID 응답 -> 프론트엔드 목록 또는 상세 화면 갱신`

이미지 기반 식재료 등록:

`프론트엔드 이미지 업로드 -> Scan API -> AI classification 호출 -> 백엔드 category 후보 조립 -> 프론트엔드 확인 화면 -> Item 생성 API`

영수증 OCR 등록:

`프론트엔드 영수증 이미지 업로드 -> Scan API -> AI OCR 호출 -> 품목명 목록 응답 -> 프론트엔드 확인 화면 -> Item 생성 API`

JSON 문법은 기본 확인으로 아래 명령을 사용합니다.

```sh
find api/examples -name '*.json' -print0 | xargs -0 -n1 python3 -m json.tool >/dev/null
```

## Item Consume API

소비 처리 API는 ACTIVE 상태의 식재료를 CONSUMED 상태로 변경합니다.

| Flow | Method | Path | Example |
|---|---|---|---|
| consume | `PATCH` | `/api/v1/items/{itemId}/consume` | [item/consume.json](examples/frontend-backend/item/consume.json) |

ACTIVE 이외 상태에서 호출 시 409 CONFLICT를 반환합니다.

삭제(DELETE)는 ACTIVE 상태를 DISCARDED로 변경합니다.

## Analytics API

분석 API는 식재료 유통기한 현황, 카테고리별 통계, 폐기율을 제공합니다.

| Flow | Method | Path | Auth | Example |
|---|---|---|---|---|
| summary | `GET` | `/api/v1/analytics/summary` | 필요 | [analytics/summary.json](examples/frontend-backend/analytics/summary.json) |
| expiring-items | `GET` | `/api/v1/analytics/expiring-items` | 필요 | [analytics/expiring-items.json](examples/frontend-backend/analytics/expiring-items.json) |

### Summary

카테고리별 활성/긴급 수량, 폐기율, 긴급 항목 목록을 한 번에 반환합니다.

카테고리는 4개 DisplayCategory로 고정됩니다.

- `VEGETABLE_FRUIT`: 채소/과일
- `DAIRY_DRINK`: 유제품
- `MEAT_SEAFOOD`: 육류/수산물
- `ETC`: 기타

urgentCount는 D-0 ~ D-3 범위이며 만료된 항목은 제외합니다. urgentItems는 최대 10개입니다.

### Expiring Items

유통기한 임박 및 만료 식재료 목록을 반환합니다.

- `maxDDay`: 기본값 10, 최대 30. D-Day 이내 항목 조회.
- `storageType`: FRIDGE, FREEZER, PANTRY. 선택 필터.
- dDay가 음수인 항목은 이미 만료된 식재료입니다 (의도적 포함).

## Tips API

보관 팁과 분리배출 가이드를 제공하는 공개 API입니다. 인증 불필요.

| Flow | Method | Path | Example |
|---|---|---|---|
| storage | `GET` | `/api/v1/tips/storage` | [tips/storage.json](examples/frontend-backend/tips/storage.json) |
| recycling | `GET` | `/api/v1/tips/recycling` | [tips/recycling.json](examples/frontend-backend/tips/recycling.json) |

보관 팁은 `category` 쿼리 파라미터로 필터링할 수 있습니다. 미입력 시 전체 반환.

## Inquiry API

문의하기 API는 관리자 이메일로 문의/신고를 발송합니다.

| Flow | Method | Path | Auth | Example |
|---|---|---|---|---|
| create | `POST` | `/api/v1/inquiries` | 필요 | [inquiry/create.json](examples/frontend-backend/inquiry/create.json) |

multipart/form-data로 type, category, content, image(선택)를 전송합니다.

## Legal API

이용약관/개인정보처리방침 URL 조회와 동의 처리 API입니다.

| Flow | Method | Path | Auth | Example |
|---|---|---|---|---|
| info | `GET` | `/api/v1/legal` | 불필요 | [legal/info.json](examples/frontend-backend/legal/info.json) |
| agreement (조회) | `GET` | `/api/v1/legal/agreement` | 필요 | [legal/agreement-get.json](examples/frontend-backend/legal/agreement-get.json) |
| agreement (동의) | `POST` | `/api/v1/legal/agreement` | 필요 | [legal/agreement-post.json](examples/frontend-backend/legal/agreement-post.json) |

약관 URL은 노션 공개 페이지를 가리킵니다. 내용 수정 시 노션에서만 편집하면 재배포 없이 반영됩니다.

## App Version API

앱 버전 체크 API는 강제/선택 업데이트를 판단합니다. 인증 불필요.

| Flow | Method | Path | Example |
|---|---|---|---|
| version | `GET` | `/api/v1/app/version` | [app/version.json](examples/frontend-backend/app/version.json) |

앱 시작 시 호출하여 `forceUpdate`가 true이면 `minimumVersion` 미만 사용자를 업데이트 화면으로 안내합니다.

## Open Questions

- Catalog 매핑 실패 시 `catalogId`, `category`, `emoji` fallback을 어떻게 내려줄 것인가?
- 생성/수정 DTO의 `storageType` 전환을 어느 PR에서 적용할 것인가?
- 현재 구현 응답의 `storage` 필드명을 목표 계약의 `storageType`으로 바꿀 것인가?
- 인증 실패와 토큰 갱신 응답 코드는 User/Auth API 기준으로 어떻게 맞출 것인가?
