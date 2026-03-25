---
name: xiaohongshu-x-planet
description: Run the "小红书运营来自 X 星球" workflow as a gstack-style command runbook. Use when the user wants to: (1) collect one or more X/Twitter accounts, (2) keep the latest N posts per account with replace-latest dedupe, (3) rebuild or inspect the local operations dashboard, (4) generate a Xiaohongshu note draft from a bundle of multiple X posts, (5) review drafts in the writable dashboard, (6) generate publish-ready payloads, (7) record published results, or (8) maintain the full X → structured data → note draft → review → publish traceability pipeline.
---

Use the implementation project at:

- GitHub: `https://github.com/xiangbaqiu/xiaohongshu-yunying-laizi-x-xingqiu`
- Local default path: `/Users/catchword/projects/ai/repos/skills/xiaohongshu-yunying-laizi-x-xingqiu`

# Runbook

Use this skill like a gstack runbook: short commands, fixed entrypoints, explicit verification.

## Command 1 — Collect

Goal: collect multiple X accounts and keep the latest N posts per account.

1. Edit `collect.config.json`
2. Prefer this shape:

```json
{
  "accounts": ["sama", "elonmusk"],
  "count_per_account": 20,
  "max_scroll_rounds": 6,
  "mode": "replace_latest"
}
```

3. Run:

```bash
cd /Users/catchword/projects/ai/repos/skills/xiaohongshu-yunying-laizi-x-xingqiu
node src/auto_collect.js collect.config.json
```

4. Expect:
   - raw batches written
   - `posts.jsonl` updated
   - `state.json` updated
   - dashboard data rebuilt

## Command 2 — Verify collection

Goal: confirm the collector met the requested target.

Run:

```bash
cd /Users/catchword/projects/ai/repos/skills/xiaohongshu-yunying-laizi-x-xingqiu
wc -l data/x/accounts/*/posts.jsonl
cat data/x/accounts/<handle>/state.json
```

Pass criteria:

- each account has `stored_post_count <= count_per_account`
- each account `mode` matches requested mode
- `latest_limit` matches requested count
- `tweet_id` uniqueness is preserved by pipeline merge rules

## Command 3 — Generate note draft

Goal: turn a bundle of multiple X posts into one Xiaohongshu draft.

1. Edit `note.config.json`
2. Prefer this shape:

```json
{
  "theme": "AI coding",
  "accounts": ["sama", "elonmusk"],
  "top_k": 4,
  "original_only": true,
  "style": "trend-analysis"
}
```

3. Run:

```bash
cd /Users/catchword/projects/ai/repos/skills/xiaohongshu-yunying-laizi-x-xingqiu
node src/run_note_pipeline.js note.config.json
```

4. Expect:
   - `notes/bundles/*.json`
   - `notes/briefs/*.json`
   - `notes/drafts/*.json`
   - `notes/drafts/*.md`
   - `notes/runs/*.json`

## Command 4 — Verify note bundle

Goal: confirm note generation used a post bundle, not a single post rewrite.

Check the latest bundle file. Expect:

- `bundle.core_post`
- `bundle.supporting_posts`
- `bundle_strategy: one-core-plus-supporting-posts`
- stable `bundle_id`

Check the latest brief file. Expect:

- `narrative_structure.hook_from_core_post`
- `narrative_structure.supporting_points`
- `narrative_structure.final_takeaway`

## Command 5 — Rebuild dashboard only

Goal: refresh operator view without recollecting.

Run:

```bash
cd /Users/catchword/projects/ai/repos/skills/xiaohongshu-yunying-laizi-x-xingqiu
node scripts/build_dashboard_data.js
```

Use when:

- dashboard data looks stale
- normalization changed
- note drafts changed
- raw/account data already exists

## Command 6 — Open read-only dashboard

Goal: visually verify operator-facing output.

Run:

```bash
cd /Users/catchword/projects/ai/repos/skills/xiaohongshu-yunying-laizi-x-xingqiu
python3 -m http.server 8008
```

Open:

`http://127.0.0.1:8008/dashboard/index.html`

Check:

- KPI summary renders
- account cards render
- content table renders
- keyword filter works
- account filter works
- content type filter works
- generated note drafts render in the `生成内容` panel

## Command 7 — Open writable dashboard

Goal: run review and publish-traceability actions from the dashboard.

Run:

```bash
cd /Users/catchword/projects/ai/repos/skills/xiaohongshu-yunying-laizi-x-xingqiu
node scripts/dashboard_server.js
```

Use writable mode when the operator needs to:

- move a draft into `reviewing`
- mark `approved` / `needs_edit` / `rejected`
- persist review annotations
- generate publish-ready payloads
- record published results

## Command 8 — Generate publish-ready payload

Goal: materialize a stable handoff artifact for an approved draft.

Run:

```bash
cd /Users/catchword/projects/ai/repos/skills/xiaohongshu-yunying-laizi-x-xingqiu
node scripts/create_publish_ready.js <draft_id> --prepared-by xiangbaqiu
```

Pass criteria:

- only `approved` drafts succeed
- `notes/publish-ready/<draft_id>.json` is written
- the dashboard can show publish-ready traceability after rebuild

## Command 9 — Record published result

Goal: write a publish record and move a draft into `published`.

Run:

```bash
cd /Users/catchword/projects/ai/repos/skills/xiaohongshu-yunying-laizi-x-xingqiu
node scripts/record_publish_result.js <draft_id> --published-by xiangbaqiu --platform-url https://www.xiaohongshu.com/explore/<note_id>
```

Pass criteria:

- `notes/publish-records/*.json` is written
- source draft is updated to `review_status = published`
- dashboard traceability shows publish time and external platform URL

## Command 10 — Rebuild from existing raw

Goal: rerun normalization without recollecting from X.

Run:

```bash
cd /Users/catchword/projects/ai/repos/skills/xiaohongshu-yunying-laizi-x-xingqiu
node src/run_from_raw.js samples/raw/<handle>-raw.json <handle>
```

Use when:

- content typing changed
- metric parsing changed
- collected raw is already good enough

# Defaults

Prefer these defaults unless the user says otherwise:

- `mode: "replace_latest"`
- dashboard default post filter: `original`
- note generation uses multiple-post bundles, not single-post rewrite
- review happens before publish-ready / publish-record steps
- preserve raw batch files
- preserve content typing fields

# Invariants

Do not break these guarantees:

- multi-account input
- target count per account
- dedupe by `tweet_id`
- replace-latest mode
- dashboard rebuild after collection
- generated drafts can be shown in dashboard
- note generation remains bundle-based
- review status remains explicit and traceable in dashboard data
- publish-ready / publish-record artifacts remain local-first and recoverable
- content type values remain: `original | repost | quote | reply | unknown`

# Operator schema

Keep these fields available in normalized posts:

- `timeline_owner`
- `display_author_handle`
- `source_author_handle`
- `content_type`
- `is_pinned`
- `tweet_id`
- `created_at`
- `metrics`

Keep these structures available in note generation outputs:

- `bundle.core_post`
- `bundle.supporting_posts`
- `review_status`
- `review_annotation`
- `narrative_structure`
- `title_options`
- `body_markdown`
- `source_posts`
- `publish_ready`
- `publish_record`

# Escalation path

If the task is about structure, commands, or data contract, read:

- `references/project-map.md`

If the task is about operating conventions or safe change order, read:

- `references/ops-rules.md`
