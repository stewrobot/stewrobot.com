# Board Rework TODO (Ready-to-Start)

_Last updated: 2026-02-17 (KST)_

## Goal
Make `24/7` feel like a production-grade, always-alive board experience.

---

## P0 — Must do first

### 1) Global view count (server-side, not browser-local)
- Add `view_count` in DB schema (`posts` table)
- Increment on post detail API fetch (or dedicated endpoint)
- Return view count in list/detail API
- Remove localStorage-based view counter from landing

**Done when**
- Different users see the same view counts
- Refreshing browser does not reset counts

---

### 2) Pagination as real board pager
- Keep page size = 10
- Add numbered pagination (`1 2 3 ...`) + prev/next
- Keep current page in URL query (`?page=2`) for share/back behavior

**Done when**
- Page navigation is deterministic and bookmarkable
- Back/forward browser buttons feel natural

---

### 3) Post detail UX hardening
- Keep toolbar (`목록/글쓰기`) fixed and consistent
- Add explicit loading skeleton/spinner state
- Prevent repeated clicks while loading detail

**Done when**
- No ambiguity after clicking a title
- No accidental duplicate transitions

---

## P1 — Strongly recommended

### 4) Notice/Pinned posts
- Add `is_pinned` field
- Always render pinned posts on top
- Visual marker (e.g., 공지/NOTICE badge)

### 5) Search + filter basics
- Title/content keyword search
- Optional author filter
- Optional sort (latest / most viewed)

### 6) Comment metadata quality
- Normalize timestamp rendering
- Better empty state / posting feedback
- Keep newline rendering and content readability

---

## P2 — Polish

### 7) Board visual consistency pass
- Column widths, spacing, hover feedback consistency
- Mobile/tablet responsiveness tuning
- Align with 24/7 theme language

### 8) API docs visibility policy
- Decide: public direct link vs operator-only emphasis
- Reflect in nav/toolbar with clear intent

---

## Safety/ops checklist (parallel)

### 9) Persistence reliability
- Restore MySQL auth (`board_user`) and disable `USE_IN_MEMORY`
- PM2 restart + smoke test
- Verify durable writes across restart

### 10) Backup/restore runbook verification
- Daily backup check
- Monthly restore drill
- Quick health script for PM2 + DB + TLS

---

## Suggested execution order (next session)
1. DB persistence fix (P0 prerequisite)
2. Global view count implementation
3. Numbered pagination + URL state
4. Detail UX hardening
5. Pin/search/filter wave

---

## Quick kickoff commands
```bash
# web landing
cd /home/tesseract/.openclaw/workspace/stewrobot/apps/web-landing

# board server
cd /home/tesseract/.openclaw/workspace/stewrobot/apps/board-server
```
