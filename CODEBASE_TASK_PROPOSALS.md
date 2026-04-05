# Codebase Task Proposals

This document captures four concrete follow-up tasks found during a quick codebase review.

## 1) Typo fix task

**Title:** Fix lowercase "i" typo in LangGraph workflow caption.

**Why:** The README caption currently says "workflow i used"; this should be "workflow I used".

**Acceptance criteria:**
- Update the caption text in `README.md` to use proper capitalization.
- Verify markdown rendering still looks correct.

---

## 2) Bug fix task

**Title:** Serve streaming endpoint as SSE (`text/event-stream`) instead of `text/plain`.

**Why:** The `/stream` endpoint emits Server-Sent Events formatted payloads (`data: ...\n\n`), but the response media type is currently `text/plain`, which can break SSE client behavior.

**Acceptance criteria:**
- Change `StreamingResponse(..., media_type=...)` in `src/routers/ask.py` from `text/plain` to `text/event-stream`.
- Confirm streaming works with an SSE-compatible client.
- Add/adjust API test coverage for response headers/content type.

---

## 3) Documentation discrepancy task

**Title:** Align README Docker Compose filename with repository reality.

**Why:** The README references `docker-compose.yml`, but the repository contains `compose.yml`.

**Acceptance criteria:**
- Update the README wording to reference `compose.yml` (or add a compatibility note if both are supported).
- Ensure all setup snippets in README use the same filename convention.

---

## 4) Test improvement task

**Title:** Replace stale service-path unit tests with router/schema-focused tests that match current tree.

**Why:** Several tests import modules under `src.services.*` (e.g., `src.services.opensearch.query_builder`) that are absent in the current source tree, leading to collection failures before meaningful test execution.

**Acceptance criteria:**
- Refactor or retire tests that import non-existent modules.
- Add at least one new fast unit test targeting currently present modules (e.g., `src/routers/*` request/response behavior or `src/schemas/*` validation).
- Ensure `pytest` can collect tests without import-path failures in a freshly synced dev environment.
