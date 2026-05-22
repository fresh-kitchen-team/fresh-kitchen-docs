# Roadmap

Status: draft
Updated: 2026-05-22

## Purpose

이 문서는 FreshKitchen 12~16주차 파트 협업 ToDo를 정리합니다.

완료 이력을 길게 보관하기보다, 각 주차에 어떤 작업 단위를 먼저 맞춰야 하는지에 집중합니다.

## 12주차

### 1. DTO 업데이트

- 프론트 할 일:
  - Item 생성/수정 request DTO에서 `catalogId`를 보내지 않도록 API 호출 값을 맞춥니다.
  - Item 생성/수정 request DTO에서 `storageId` 대신 `storageType`을 보내도록 등록/수정 흐름을 맞춥니다.
  - `storageType` 공개 값은 `FRIDGE`, `FREEZER`, `PANTRY`를 사용합니다.
  - 프론트-백엔드 request 예시에서 `catalogId`, `storageId`가 새로 노출되지 않는지 확인합니다.
- 백엔드 할 일:
  - `name` 또는 AI/OCR 인식 결과명을 catalog master data에 매핑합니다.
  - category, catalog, 유통기한 자동 매핑 서비스를 구현합니다.
  - `storageType`을 인증 사용자 기준 내부 `storageId`로 매핑합니다.
  - Item 조회/list/detail response DTO에는 `catalogId`, `category`, `emoji`를 포함합니다.
  - classification 응답 루트에 백엔드 category 코드를 보강합니다.
  - OCR 품목별 DTO에 `items[].category`를 보강합니다.
  - AI 서버 원본 응답과 백엔드 보강 DTO의 차이를 문서와 예시에 반영합니다.
  - `api/frontend-backend.md`와 `api/backend-ai.md`의 목표 계약이 같은 기준을 쓰는지 확인합니다.
  - API 예시는 도메인 폴더와 흐름별 JSON으로 관리하고, 한 파일 안에 `endpoint`, `request`, `response`를 함께 작성합니다.
- AI 할 일:
  - classification 응답의 `source` 값을 유지하고 가능한 값을 확정합니다.
  - classification/OCR 결과에서 category 매핑에 필요한 식재료명 값을 안정적으로 내려줄 수 있는지 확인합니다.
  - `source`, `category` 설명이 AI 서버 원본 응답과 백엔드 보강 DTO를 구분하는지 확인합니다.

### 2. 온보딩과 인증 진입

- 프론트 할 일:
  - 온보딩 화면을 준비합니다.
  - 카카오/구글 로그인 진입 화면을 준비합니다.
  - 온보딩 안에서 동의약관 렌더링 화면을 준비합니다.
- 백엔드 할 일:
  - 카카오/구글 로그인 구현 완료 상태를 프론트 로그인 진입 흐름과 연결해 설명할 수 있게 정리합니다.
  - 로그아웃 구현 범위를 정리합니다.

### 3. 시스템 알림 메일 흐름

- 프론트 할 일:
  - 시스템 알림 메일 발송 후 사용자가 확인할 알림 상태 화면이 필요한지 검토합니다.
  - 메일 수신 후 사용자가 돌아올 복귀 화면이나 알림 히스토리 화면이 필요한지 검토합니다.
- 백엔드 할 일:
  - Java Mail Sender로 시스템이 유저에게 이상 상황 메일을 보내는 흐름을 설명할 수 있게 정리합니다.

### 4. AI 채팅과 검색 흐름

- 프론트 할 일:
  - AI 채팅 화면에서 식재료 기반 레시피 추천 질문 흐름을 준비합니다.
  - AI 채팅 화면에서 일반 검색 질문 흐름을 준비합니다.
- AI 할 일:
  - 냉장고 내 식재료 기반 레시피 추천 흐름을 준비합니다.
  - 일반 LLM 웹 검색 연동 질의 흐름을 준비합니다.

### 5. 유통기한 빠른 입력

- 프론트 할 일:
  - 유통기한 설정 화면에 `D-1`, `D-3`, `D-5`, `D-10` 빠른 입력 버튼을 준비합니다.
  - 버튼이 API 필드가 아니라 날짜 입력 편의 기능이라는 점을 발표 설명에 맞춥니다.

### 6. 냉장고 detection과 분석/팁 연결

- 백엔드 할 일:
  - ~~분석/팁 화면에 연결할 백엔드 응답 또는 라우팅 흐름을 준비합니다.~~ (완료: Analytics API + Tips API 구현)
- AI 할 일:
  - 냉장고 detection 결과를 백엔드와 연결하는 작업을 시작합니다.

## 13주차

### 1. SCAN 서비스 1차 연결 유지

프론트엔드, 백엔드, AI 서버를 연결해 사용자가 스캔 결과를 확인하고 저장까지 이어갈 수 있는 흐름을 맞춥니다.

개발 범위를 늘리지 않기 위해 이미지 업로드 방식은 현재 multipart 기반 SCAN 흐름을 유지하고, presigned URL 리팩토링은 이번 단계에서 진행하지 않습니다.

- 프론트 할 일:
  - classification, receipt OCR, fridge detection 진입 화면과 결과 확인 화면을 백엔드 SCAN API에 연결합니다.
  - 스캔 결과는 바로 저장하지 않고, 사용자가 `recognizedItems`를 확인하거나 수정한 뒤 저장하도록 흐름을 맞춥니다.
  - classification 결과 화면에서는 인식된 식재료 후보와 함께 업로드한 실물 사진을 유저가 볼 수 있게 표시합니다.
  - presigned URL 업로드 전환을 위한 새 화면 상태나 업로드 단계를 추가하지 않습니다.
- 백엔드 할 일:
  - SCAN API가 AI 서버 classification, receipt OCR, fridge detection 엔드포인트를 호출하는 흐름을 연결합니다.
  - AI 서버 원본 응답을 프론트 확인 화면에서 사용할 DTO로 변환합니다.
  - classification 응답의 `recognizedItems`에 실물 사진 참조값을 포함합니다.
  - receipt OCR과 fridge detection 응답은 기본 표시값으로 `emoji`를 내려줍니다.
  - AI 응답의 식재료명 또는 class label을 catalog/category/emoji 매핑에 사용할 수 있게 정규화합니다.
  - SCAN API와 Item 저장 API의 파일 전달 방식은 현재 multipart 흐름을 유지합니다.
- AI 할 일:
  - classification, receipt OCR, fridge detection 응답에서 백엔드가 후보명으로 사용할 값을 안정적으로 내려줍니다.
  - classification은 실물 사진과 후보 식재료가 같은 결과 항목으로 연결될 수 있게 응답 기준을 확인합니다.
  - receipt OCR과 fridge detection은 품목명이 emoji 매핑에 사용할 수 있을 만큼 일관적인지 확인합니다.

### 2. 식재료 매핑 서비스화와 catalog 운영 데이터 보강

식재료명, AI classification label, OCR 인식 결과를 백엔드 운영 데이터에 안정적으로 매핑할 수 있게 정리합니다.

- 프론트 할 일:
  - Item 생성/수정 요청에서 `catalogId`를 직접 선택하거나 전송하지 않습니다.
  - 사용자가 입력한 식재료명, 저장 위치, 유통기한 날짜를 백엔드 계약에 맞춰 보냅니다.
  - 유통기한 빠른 입력 버튼은 날짜 계산 편의 기능으로 유지하고, 실제 저장값은 계산된 날짜로 맞춥니다.
  - 백엔드가 내려주는 `category`, `emoji`, `image` 값을 화면 표시 기준으로 사용합니다.
- 백엔드 할 일:
  - 식재료 유통기한 자동 매핑을 서비스 단위로 분리합니다.
  - 수동 입력명, AI classification label, OCR 인식명을 catalog/category/emoji 매핑 서비스에서 처리합니다.
  - catalog 운영 데이터를 AI classification 목표 70개 class에 맞게 추가합니다.
  - 운영 데이터에 없는 식재료명은 fallback category와 기본 emoji를 내려줄 수 있게 기준을 정합니다.
  - 유통기한 매핑 실패 시 프론트가 직접 입력한 날짜를 우선 사용할 수 있게 저장 기준을 정리합니다.
- 공통/통합 할 일:
  - classification 70개 class label과 catalog 운영 데이터가 발표용 시나리오에서 어긋나지 않는지 확인합니다.
  - 수동 입력, classification, OCR 저장 흐름이 같은 catalog/category/emoji 기준을 쓰는지 확인합니다.

### 3. image와 emoji primary 표시 제약

Item 화면 표시값은 `image`와 `emoji` 중 하나가 primary가 되도록 제약을 정합니다.

- 프론트 할 일:
  - 응답에 `image`가 있으면 이미지 썸네일을 primary로 표시합니다.
  - 응답에 `image`가 없으면 `emoji`를 primary로 표시합니다.
  - receipt OCR과 fridge detection으로 저장한 Item은 기본적으로 emoji 표시를 사용합니다.
  - 목록, 상세, 홈/요약 화면에서 같은 표시 규칙을 사용합니다.
- 백엔드 할 일:
  - classification 저장 Item은 image primary가 될 수 있게 실물 사진 참조값을 연결합니다.
  - OCR, fridge detection, manual 저장 Item은 기본적으로 emoji primary가 되도록 응답을 맞춥니다.
  - image가 없는 Item은 catalog/category 기준 emoji fallback이 반드시 내려가게 합니다.
  - Item list/detail/home summary/analytics preview 응답에서 표시값 기준이 흔들리지 않게 맞춥니다.
- AI 할 일:
  - AI 응답의 후보명이 백엔드 emoji 매핑 테이블과 맞물릴 수 있게 불필요한 설명 문장보다 식재료명 중심으로 응답합니다.

## 14주차

### 1. 개발 축소와 안정성 확인

14주차는 새 업로드 구조를 추가하지 않고, 13주차에 맞춘 매핑과 표시 정책이 실제 화면에서 안정적으로 보이는지 확인합니다.

- 프론트 할 일:
  - 기존 multipart 이미지 업로드와 SCAN 화면 흐름을 유지합니다.
  - upload, scan, save 실패 상태를 사용자가 구분할 수 있게 문구와 화면 상태를 점검합니다.
  - 새 기능 추가보다 발표에서 보여줄 입력, 확인, 저장, 조회 흐름을 안정화합니다.
- 백엔드 할 일:
  - presigned URL 발급 API와 업로드 구조 전환은 진행하지 않습니다.
  - 유통기한 매핑 서비스와 catalog 70개 운영 데이터가 실제 저장 흐름에서 동작하는지 검증합니다.
  - 매핑 실패, image 누락, emoji 누락 상황에서 fallback 응답이 유지되는지 확인합니다.
- 공통/통합 할 일:
  - 개발 범위를 줄이고 안정성 테스트, 광고/배포 심사, 사용자 편의성, 발표 완성도에 집중합니다.
  - API 문서와 예시 JSON은 실제 유지할 multipart 흐름과 표시 정책 기준으로만 갱신합니다.

### 2. SCAN 엔드포인트별 이미지 범위 정리

각 SCAN 엔드포인트에서 이미지가 어디까지 저장과 표시로 이어지는지 범위를 분리합니다.

- classification:
  - 실물 사진을 `recognizedItems`에 포함합니다.
  - 사용자가 저장하면 실물 사진이 Item 대표 이미지가 됩니다.
  - 이후 목록, 상세, 홈/요약 등 GET 응답에서 실물 사진이 썸네일로 내려옵니다.
- receipt OCR:
  - 기본 표시값은 `emoji`입니다.
  - 영수증 원본 사진을 Item별 대표 이미지로 나누어 저장하는 기능은 추후 범위로 둡니다.
- fridge detection:
  - 기본 표시값은 `emoji`입니다.
  - 냉장고 전체 사진 또는 crop 이미지를 Item별 대표 이미지로 저장하는 기능은 추후 범위로 둡니다.

### 3. GET 응답 썸네일 일관성 검증

저장된 Item을 어떤 화면에서 조회해도 같은 표시 정책을 따르는지 확인합니다.

- 프론트 할 일:
  - 목록, 상세, 홈/요약 화면에서 `image` 우선, `emoji` fallback 표시가 같은 기준으로 동작하는지 확인합니다.
  - image 로딩 실패 시 화면이 깨지지 않고 emoji 또는 기본 표시값으로 내려갈 수 있게 처리합니다.
  - classification, OCR, fridge detection, manual 저장 Item을 섞어 조회해도 표시 규칙이 달라지지 않는지 확인합니다.
- 백엔드 할 일:
  - Item list/detail/home summary/analytics preview 등 Item 썸네일을 포함하는 응답의 필드 기준을 맞춥니다.
  - classification 저장 Item은 image primary, OCR/detection 저장 Item은 emoji primary가 되는지 통합 테스트합니다.
  - image가 없는 Item에 대해 emoji 매핑이 누락되지 않는지 확인합니다.
  - catalog 70개 운영 데이터와 유통기한 매핑 서비스 결과가 조회 응답에 일관되게 반영되는지 확인합니다.
- 공통/통합 할 일:
  - classification으로 저장한 식재료와 OCR/detection으로 저장한 식재료를 섞어 조회하는 시나리오를 검증합니다.
  - API 문서와 예시 JSON을 실제 응답 기준으로 갱신할 항목을 정리합니다.

## 15주차

## 16주차

## Update Rule

로드맵은 완료 이력을 길게 보관하지 않습니다.

단계가 끝나면 현재 기준으로 내용을 갱신하고, 다음에 확인해야 할 작업만 남깁니다.

진행 상태는 `STATUS.md`에서 관리합니다.

`ROADMAP.md`는 12~16주차 예정 ToDo에 집중하고, `STATUS.md`는 지금 완료, 진행, 막힘, 다음 작업에 집중합니다.
