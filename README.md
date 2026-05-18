# FreshKitchen Docs

FreshKitchen 팀이 지금 당장 확인해야 하는 문서를 바로 찾도록 관리하는 저장소입니다.

이 저장소는 오래된 문서를 쌓아두는 곳이 아니라, 팀원이 현재 상태를 빠르게 확인하고 같은 기준으로 작업하기 위한 문서판입니다. 원하는 내용을 찾기 위해 여러 파일을 뒤지지 않도록, 목적별로 볼 파일을 분명히 나눕니다.

문서가 바뀌면 새 파일을 계속 늘리기보다 현재 기준 문서를 직접 갱신합니다.

## 무엇을 찾고 있나요?

지금 프로젝트가 어디까지 왔는지, 무엇이 진행 중이고 무엇이 막혔는지 확인하려면 [STATUS.md](STATUS.md)를 봅니다.

이번 단계나 최종 발표에 포함되는 기능과 제외되는 기능을 확인하려면 [SCOPE.md](SCOPE.md)를 봅니다.

12~16주차에 어떤 작업을 먼저 맞춰야 하는지, 파트별 ToDo가 궁금하면 [ROADMAP.md](ROADMAP.md)를 봅니다.

발표를 준비하거나 시연 순서, 스토리보드, 체크리스트가 필요하면 [PRESENTATION.md](PRESENTATION.md)에서 해당 주차 발표 문서로 이동합니다.

프론트엔드와 백엔드가 맞춰야 하는 DTO, API 경로, 요청/응답 예시가 필요하면 [api/frontend-backend.md](api/frontend-backend.md)를 봅니다.

백엔드가 AI 서버를 언제 어떻게 호출하는지, AI 응답과 실패 조건을 맞춰야 하면 [api/backend-ai.md](api/backend-ai.md)를 봅니다.

API 예시 JSON을 어디에 어떤 형식으로 작성해야 하는지 확인하려면 [api/README.md](api/README.md)를 봅니다.

AI agent로 문서를 수정하거나 커밋 규칙을 확인해야 하면 [AGENTS.md](AGENTS.md)를 봅니다.

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
