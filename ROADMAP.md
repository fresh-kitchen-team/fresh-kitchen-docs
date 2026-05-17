# Roadmap

Status: draft
Updated: 2026-05-17

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
  - 분석/팁 화면에 연결할 백엔드 응답 또는 라우팅 흐름을 준비합니다.
- AI 할 일:
  - 냉장고 detection 결과를 백엔드와 연결하는 작업을 시작합니다.

## 13주차

### 1. 냉장고 detection 프론트-백엔드 연동

- 프론트 할 일:
  - 냉장고 detection 화면으로 이동할 수 있는 네비게이션을 추가합니다.
  - 냉장고 detection 요청과 결과 표시 흐름을 백엔드 API와 연결합니다.
- 백엔드 할 일:
  - 냉장고 detection 프론트 요청을 받을 API 흐름을 정리합니다.
  - AI detection 결과를 프론트에서 사용할 응답 DTO로 내려줍니다.

## 14주차

## 15주차

## 16주차

## Update Rule

로드맵은 완료 이력을 길게 보관하지 않습니다.

단계가 끝나면 현재 기준으로 내용을 갱신하고, 다음에 확인해야 할 작업만 남깁니다.

진행 상태는 `STATUS.md`에서 관리합니다.

`ROADMAP.md`는 12~16주차 예정 ToDo에 집중하고, `STATUS.md`는 지금 완료, 진행, 막힘, 다음 작업에 집중합니다.
