# Project map

Project root:

`/Users/xiangbaqiu/.openclaw/workspace-cto-agent/xiaohongshu-yunying-laizi-x-xingqiu`

## Main commands

### Auto collect multiple accounts

```bash
cd /Users/xiangbaqiu/.openclaw/workspace-cto-agent/xiaohongshu-yunying-laizi-x-xingqiu
node src/auto_collect.js collect.config.json
```

### Generate note draft from a post bundle

```bash
cd /Users/xiangbaqiu/.openclaw/workspace-cto-agent/xiaohongshu-yunying-laizi-x-xingqiu
node src/run_note_pipeline.js note.config.json
```

### Rebuild dashboard data only

```bash
cd /Users/xiangbaqiu/.openclaw/workspace-cto-agent/xiaohongshu-yunying-laizi-x-xingqiu
node scripts/build_dashboard_data.js
```

### Serve dashboard locally

```bash
cd /Users/xiangbaqiu/.openclaw/workspace-cto-agent/xiaohongshu-yunying-laizi-x-xingqiu
python3 -m http.server 8008
```

Open:

`http://127.0.0.1:8008/dashboard/index.html`

## Important configs

### `collect.config.json`

```json
{
  "accounts": ["sama", "elonmusk"],
  "count_per_account": 3,
  "max_scroll_rounds": 4,
  "mode": "replace_latest"
}
```

### `note.config.json`

```json
{
  "theme": "AI coding",
  "accounts": ["sama", "elonmusk"],
  "top_k": 4,
  "original_only": true,
  "style": "trend-analysis"
}
```

## Data contract

### Normalized post

```json
{
  "platform": "x",
  "author_handle": "elonmusk",
  "timeline_owner": "elonmusk",
  "display_author_handle": "elonmusk",
  "source_author_handle": "elonmusk",
  "content_type": "original",
  "is_pinned": false,
  "tweet_id": "2035526376468394305",
  "url": "https://x.com/elonmusk/status/2035526376468394305",
  "text": "...",
  "created_at": "2026-03-22T01:18:01.000Z",
  "metrics": {
    "reply": 3834,
    "repost": 6833,
    "like": 31026,
    "view": 11097829
  },
  "media_urls": [],
  "fetched_at": "2026-03-23T04:06:38.704Z",
  "source": {
    "method": "openclaw-browser",
    "version": "mvp-v0.4.1-auto"
  }
}
```

### Selection bundle

```json
{
  "selection_id": "sel-...",
  "theme": "AI coding",
  "bundle_strategy": "one-core-plus-supporting-posts",
  "bundle": {
    "core_post": {
      "tweet_id": "2035526376468394305",
      "account": "elonmusk"
    },
    "supporting_posts": [
      {
        "tweet_id": "2033935276079510011",
        "account": "sama",
        "support_role": "comparison"
      }
    ]
  }
}
```

### Brief

```json
{
  "brief_id": "brief-...",
  "theme": "AI coding",
  "core_angle": "...",
  "narrative_structure": {
    "hook_from_core_post": "...",
    "supporting_points": [],
    "final_takeaway": "..."
  }
}
```

### Note draft

```json
{
  "note_id": "note-...",
  "theme": "AI coding",
  "style": "trend-analysis",
  "title_options": ["..."],
  "body_markdown": "...",
  "hashtags": ["#AI"],
  "source_posts": []
}
```

## Merge modes

### `append`
Append new normalized posts to existing ones, then dedupe by `tweet_id`.

### `replace_latest`
Merge + dedupe + sort newest-first, then keep only latest N posts per account.

Preferred mode: `replace_latest`

## Output paths

### Collection outputs

- `data/x/accounts/<handle>/posts.jsonl`
- `data/x/accounts/<handle>/state.json`
- `data/x/accounts/<handle>/raw-batches/<runId>.json`
- `data/runs/<runId>/summary.json`
- `data/dashboard/dashboard-data.json`

### Note outputs

- `notes/selections/*.json`
- `notes/briefs/*.json`
- `notes/drafts/*.json`
- `notes/drafts/*.md`

### Dashboard note surface

`data/dashboard/dashboard-data.json` now includes a `notes` array for generated drafts, and the dashboard renders those drafts in the `生成内容` panel.
