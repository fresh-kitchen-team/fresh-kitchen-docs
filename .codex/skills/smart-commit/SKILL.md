---
name: smart-commit
description: Analyze documentation changes, split them into readable commit units, and create Korean commits for the FreshKitchen docs repository. Stop before push.
---

# Smart Commit Skill

Root entrypoint: `AGENTS.md`.

Use this skill when creating commits in the FreshKitchen docs repository.

This repository is a documentation repository. Commit grouping and messages should optimize for readable document history, not application code history.

## Purpose

This skill creates high-quality documentation commits.

It performs:

1. git status inspection
2. git diff analysis
3. document-oriented grouping
4. selective staging
5. Korean commit message generation
6. commit execution

This skill MUST NOT:

- push changes
- create pull requests
- merge branches
- rewrite unrelated user changes

## Execution Flow

Follow this order strictly:

1. inspect current branch
2. run `git status`
3. run `git diff`
4. identify modified, added, and deleted documents
5. group changes by document purpose
6. present a short commit plan when there is more than one logical unit
7. stage only the files or hunks for one unit
8. create a Korean commit message
9. run `git commit`
10. repeat for remaining units when clean separation exists
11. stop before push

## Commit Message Format

Use this format:

```text
Type(Scope) : Korean summary
```

Examples:

```text
Docs(README) : 문서 저장소 목적 정리
Docs(STATUS) : 현재 진행 상태 초안 작성
Update(v0.2) : 발표 범위와 시연 흐름 반영
Fix(API) : 백엔드-AI 응답 예시 수정
Remove(Scope) : 제외된 기능 설명 제거
Chore(Repo) : 문서 작업 규칙 정리
```

## Allowed Types

- `Docs`: 새 문서 추가 또는 주요 내용 작성
- `Update`: 기존 문서의 상태, 범위, 일정, 버전 갱신
- `Fix`: 잘못된 설명, 링크, 예시 수정
- `Remove`: 오래된 내용 삭제
- `Chore`: 문서 저장소 운영 규칙 또는 설정 정리

## Scope Rules

Use a short scope that helps readers understand which document area changed.

Good scopes:

- `README`
- `STATUS`
- `SCOPE`
- `ROADMAP`
- `Presentation`
- `API`
- `Repo`
- `v0.1`
- `v0.2`

Avoid code-domain scopes such as `User`, `Auth`, `Service`, or `Controller` unless the document itself is specifically about that API area.

## Language Rules

- Write the summary in Korean.
- Keep the summary short and readable.
- Do not end the summary with a period.
- Prefer the document result over the editing action.

Good:

```text
Docs(SCOPE) : 발표 포함 범위 초안 작성
Fix(API) : 프론트엔드 응답 예시 링크 수정
Update(v0.3) : API 계약 정렬 단계 반영
```

Bad:

```text
Docs(SCOPE) : Add scope document
Fix(API) : 오타 수정함.
Update(User) : user service update
```

## Grouping Rules

One commit should represent one documentation intention.

Valid units:

- repository purpose and navigation
- current status update
- scope update
- roadmap or version update
- presentation and demo update
- API contract update
- example payload update
- AI working rule update

Do not mix unrelated intentions when they can be separated cleanly.

Examples:

- `README.md` and `AGENTS.md` can be committed together when both explain repository operation.
- `api/frontend-backend.md` and its matching example JSON can be committed together.
- `STATUS.md` and `ROADMAP.md` should be separate if one updates progress and the other changes milestone direction.

## Selective Staging Rules

Prefer file-level staging when each file has one clear purpose.

Use hunk-level staging only when a single file contains unrelated document changes.

Avoid `git add .` unless all changes are clearly one documentation unit.

## Commit Body Rules

Commit body is optional.

Use a body only when the reason is not obvious from the title.

When writing a body:

- write in Korean
- mention what changed
- mention why it changed
- keep it to a few short lines

## Final Report

After committing, report:

- commit hash
- commit message
- files included
- anything intentionally left uncommitted
