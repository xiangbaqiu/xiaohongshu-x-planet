# xiaohongshu-x-planet

An OpenClaw skill for running the **X/Twitter → structured data → Xiaohongshu draft → dashboard** workflow.

## What it does

- Collect one or more X accounts
- Keep the latest N posts per account with `replace_latest` dedupe
- Rebuild or inspect the local operations dashboard
- Generate a Xiaohongshu note draft from a bundle of multiple X posts
- Show generated drafts inside the dashboard
- Maintain the full X → normalized posts → note draft → dashboard pipeline

## Skill entry

See [SKILL.md](./SKILL.md).

## Project root used by this skill

`/Users/xiangbaqiu/.openclaw/workspace-cto-agent/xiaohongshu-yunying-laizi-x-xingqiu`

## Main commands

### Collect

```bash
cd /Users/xiangbaqiu/.openclaw/workspace-cto-agent/xiaohongshu-yunying-laizi-x-xingqiu
node src/auto_collect.js collect.config.json
```

### Generate note draft

```bash
cd /Users/xiangbaqiu/.openclaw/workspace-cto-agent/xiaohongshu-yunying-laizi-x-xingqiu
node src/run_note_pipeline.js note.config.json
```

### Rebuild dashboard

```bash
cd /Users/xiangbaqiu/.openclaw/workspace-cto-agent/xiaohongshu-yunying-laizi-x-xingqiu
node scripts/build_dashboard_data.js
```

### Open dashboard

```bash
cd /Users/xiangbaqiu/.openclaw/workspace-cto-agent/xiaohongshu-yunying-laizi-x-xingqiu
python3 -m http.server 8008
```

Then open:

`http://127.0.0.1:8008/dashboard/index.html`

## Included references

- `references/project-map.md`
- `references/ops-rules.md`

## Notes

This repository contains the **skill definition and operator references**. It does **not** include the full implementation project unless you copy or publish that separately.
