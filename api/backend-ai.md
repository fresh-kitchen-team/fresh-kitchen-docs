# Backend-AI API

Status: draft
Updated: 2026-05-17

## Purpose

이 문서는 FreshKitchen 백엔드와 AI 서버 사이의 API 계약을 정리합니다.

백엔드 팀원은 어떤 시점에 AI 서버를 호출하는지 확인하고, AI 팀원은 요청과 응답 형식을 맞춥니다.

## Current State

아직 실제 AI 서버 API가 모두 반영되지 않았습니다.

현재 문서는 AI 서버 README의 원본 응답을 기준으로 확인하되, 백엔드가 사용할 보강 DTO를 함께 설명합니다.

AI 서버 원본 응답과 백엔드 보강 DTO가 다르면 본문에 차이를 남깁니다.

## Call Direction

백엔드가 AI 서버에 요청을 보냅니다.

AI 서버는 백엔드 요청에 대한 분석 결과나 추천 결과를 응답합니다.

현재 백엔드 코드 기준으로 AI 서버 호출은 multipart file 요청을 사용합니다.

주요 경로는 아래와 같습니다.

- `/internal/v1/food-classification`
- `/internal/v1/receipt-ocr`
- `/internal/v1/fridge-detection`

## Common Conditions

AI 서버의 모든 엔드포인트는 Bearer token 인증을 사용합니다.

```http
Authorization: Bearer <AI_SECRET_TOKEN>
```

파일 업로드 공통 조건은 아래와 같습니다.

- 요청 형식: `multipart/form-data`
- 파일 파트 이름: `file`
- 최대 파일 크기: 10 MB
- 허용 Content-Type: `image/jpeg`, `image/png`, `image/webp`

공통으로 확인해야 하는 오류는 아래와 같습니다.

- `401`: 인증 토큰 불일치
- `413`: 파일 크기 초과
- `415`: 허용되지 않는 파일 형식
- `500`: AI 서버 내부 처리 실패

## AI Response And Backend DTO

AI 서버 README는 원본 AI 응답 필드를 camelCase 또는 단순 배열로 설명합니다.

백엔드는 프론트엔드와 저장 로직에서 쓰기 쉽게 응답을 보강하거나 정규화할 수 있습니다.

- classification 원본: `bestMatch`, `confidence`, `top3`, `source`
- classification 백엔드 DTO: `best_match`, `category`, `confidence`, `top_3`, `source`
- OCR 원본: `purchasedAt`, `ingredients`
- OCR 백엔드 DTO: `recognized_text`, `items[].name`, `items[].quantity`, `items[].category`
- fridge detection 원본: `items`
- fridge detection 백엔드 DTO: `detected_items[]`

`category`는 AI 서버 원본 응답이 아니라 백엔드가 사용하는 보강 필드입니다.

## Classification Source Values

classification 응답의 `source`는 음식 분류 결과가 어느 경로에서 나왔는지 나타냅니다.

AI 서버 README 기준 값은 아래와 같습니다.

- `efficientnet`: EfficientNet이 75% 이상 확신도로 분류했습니다.
- `gemini`: EfficientNet 확신도가 75% 미만이라 Gemini 2.5 Flash가 분류했습니다.
- `efficientnet_fallback`: EfficientNet 확신도가 75% 미만이고 Gemini 호출도 실패해 EfficientNet 결과를 그대로 반환했습니다.

`gemini` source일 때는 `gemini_reason`과 `auto_saved`를 함께 확인합니다.

`auto_saved`는 Gemini 분류 성공 시 자동 레이블링용으로 저장된 이미지 경로입니다.

## Category Mapping Plan

classification과 OCR 응답 DTO에 백엔드 category를 매핑한 `category` 항목을 추가할 예정입니다.

`category` 값은 화면 표시명이 아니라 백엔드에서 사용하는 category 코드 문자열입니다.

classification 응답은 루트에 `category`를 추가합니다.

- 현재 classification class는 60개입니다.
- 목표 classification class는 70개입니다.
- AI classification의 class label 또는 `bestMatch` 값을 백엔드 category에 하드코딩으로 매핑합니다.
- 매핑 테이블은 백엔드 category 기준이 바뀌면 함께 갱신합니다.

OCR 응답은 각 인식 품목에 `category`를 추가합니다.

- `items[].category`에 품목별 백엔드 category를 넣습니다.
- OCR에서 인식한 품목 이름을 LLM으로 백엔드 category에 매핑합니다.
- 같은 영수증 안에서도 품목마다 다른 category가 올 수 있습니다.

## Failure Handling

백엔드는 AI 서버 호출 실패를 사용자 요청 전체 실패로 볼지, 대체 응답으로 처리할지 결정해야 합니다.

확인해야 할 실패 상황은 아래와 같습니다.

- AI 서버 연결 실패
- AI 서버 timeout
- AI 서버 `401` 응답
- AI 서버 `413` 응답
- AI 서버 `415` 응답
- AI 서버 `500` 응답
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

## Functional Flow Validation

`api/examples/`의 백엔드-AI 예시는 JSON 문법만 맞는지보다 백엔드 기능 흐름에서 AI 응답을 사용할 수 있는지 확인하기 위해 사용합니다.

예시는 아래 질문에 답할 수 있어야 합니다.

- 백엔드가 AI 서버에 보낼 입력 파일과 요청 정보를 확인할 수 있는가?
- AI 응답 DTO가 백엔드 저장 모델이나 다음 응답 DTO로 변환될 수 있는가?
- AI가 내려준 값과 백엔드가 매핑해야 하는 값이 구분되는가?
- `category`는 표시명이 아니라 백엔드 category 코드로 유지되는가?
- 실패가 발생하면 AI 호출 실패인지, 응답 형식 오류인지, 매핑 실패인지 구분할 수 있는가?

classification 흐름:

`백엔드 이미지 파일 요청 -> AI bestMatch, confidence, top3, source 응답 -> 백엔드가 bestMatch를 category로 매핑 -> 백엔드 저장 또는 프론트엔드 응답에서 category 사용`

검증 기준:

- 응답 루트에 `category`가 있어야 합니다.
- `best_match`는 사용자가 볼 수 있는 식재료 후보 이름입니다.
- `category`는 백엔드 저장과 필터링에 사용할 category 코드입니다.
- `source`는 `efficientnet`, `gemini`, `efficientnet_fallback` 중 하나여야 합니다.
- classification class label 또는 `bestMatch`와 백엔드 category 매핑 규칙은 하드코딩 기준으로 설명할 수 있어야 합니다.

OCR 흐름:

`백엔드 영수증 이미지 요청 -> AI purchasedAt, ingredients 응답 -> 백엔드가 품목별 DTO와 category를 보강 -> 백엔드가 품목별 category를 저장 또는 확인 응답에 사용`

검증 기준:

- AI 서버 원본 응답의 `purchasedAt`은 구매일로 사용할 수 있어야 합니다.
- AI 서버 원본 응답의 `ingredients[]`는 식재료명 배열로 해석할 수 있어야 합니다.
- 각 `items[]` 항목에 `name`, `quantity`, `category`가 있어야 합니다.
- `items[].name`은 인식된 품목명입니다.
- `items[].category`는 LLM이 백엔드 category에 매핑한 코드입니다.
- 같은 영수증 안에서 품목별 category가 달라도 처리할 수 있어야 합니다.

fridge detection 흐름:

`백엔드 냉장고 이미지 요청 -> AI items 응답 -> 백엔드가 detected_items 후보로 변환 -> 프론트엔드 확인 화면 또는 저장 흐름으로 전달`

검증 기준:

- AI 서버 원본 응답의 `items[]`는 한글 식재료명 배열로 해석할 수 있어야 합니다.
- 백엔드 보강 DTO의 `detected_items[]`는 후보 식재료 목록으로 해석할 수 있어야 합니다.
- category가 필요한 저장 흐름으로 연결된다면 별도 매핑 기준을 추가해야 합니다.

JSON 문법은 기본 확인으로만 아래 명령을 사용합니다.

```sh
find api/examples -name '*.json' -print0 | xargs -0 -n1 python3 -m json.tool >/dev/null
```

## Open Questions

- AI 서버 timeout 기준은 몇 초인가?
- AI 서버 실패 시 백엔드는 어떤 에러 코드로 응답하는가?
- 발표 시연에서 AI 서버를 실시간 호출할 것인가?
