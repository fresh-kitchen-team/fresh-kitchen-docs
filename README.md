# FreshKitchen Docs

FreshKitchen 팀이 지금 당장 확인해야 하는 문서를 관리하는 저장소입니다.

이 저장소는 오래된 문서를 쌓아두는 곳이 아니라, 팀원이 현재 상태를 빠르게 확인하고 같은 기준으로 작업하기 위한 문서판입니다. 문서가 바뀌면 새 파일을 계속 늘리기보다 루트 문서를 직접 갱신합니다.

## Start Here

- [STATUS.md](STATUS.md): 현재 진행 상태와 progress 관리 범위
- [SCOPE.md](SCOPE.md): 이번 프로젝트의 포함 범위와 제외 범위
- [ROADMAP.md](ROADMAP.md): 남은 작업 순서와 문서 버전
- [PRESENTATION.md](PRESENTATION.md): 발표 범위와 시연 흐름
- [api/frontend-backend.md](api/frontend-backend.md): 프론트엔드와 백엔드 API 계약
- [api/backend-ai.md](api/backend-ai.md): 백엔드와 AI 서버 API 계약

## How We Write

문서는 GitHub에서 Markdown 파일을 바로 열었을 때 이해 가능해야 합니다.

- 짧은 문단과 불릿을 우선합니다.
- 표는 꼭 필요한 비교나 필드 매핑에만 사용합니다.
- 확정된 내용과 논의 중인 내용을 섞지 않습니다.
- 오래된 내용은 남겨두지 말고 현재 기준으로 갱신합니다.
- 긴 JSON이나 YAML 예시는 `api/examples/`에 분리합니다.

## Document Status

문서 상태는 필요할 때 본문 상단에 짧게 적습니다.

- `draft`: 초안입니다.
- `review`: 팀 확인이 필요합니다.
- `confirmed`: 현재 기준으로 확정된 내용입니다.
- `archived`: 더 이상 현재 기준으로 사용하지 않습니다.

## Working With AI

AI agent는 [AGENTS.md](AGENTS.md)를 먼저 읽고 작업합니다.

커밋 생성이 필요하면 `.codex/skills/smart-commit/SKILL.md`를 따릅니다.
