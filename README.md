# xiaohongshu-x-planet

OpenClaw skill for the **X/Twitter → structured data → Xiaohongshu draft → dashboard** workflow.

This repository is the **skill / runbook layer**.
It defines how an agent should operate the workflow, verify results, and hand off to the implementation project.

## Repositories

- **Skill repo:** `https://github.com/xiangbaqiu/xiaohongshu-x-planet`
- **Implementation repo:** `https://github.com/xiangbaqiu/xiaohongshu-yunying-laizi-x-xingqiu`

## What this skill covers

- Collect one or more X accounts
- Keep the latest N posts per account with `replace_latest` dedupe
- Rebuild or inspect the local operations dashboard
- Generate a Xiaohongshu note draft from a bundle of multiple X posts
- Show generated drafts inside the dashboard
- Maintain the full X → normalized posts → note draft → dashboard pipeline

## What's inside this repo

- `SKILL.md` — main skill definition
- `references/project-map.md` — command map and data contract notes
- `references/ops-rules.md` — operating conventions and safe change order

## Implementation project used by this skill

- GitHub: `https://github.com/xiangbaqiu/xiaohongshu-yunying-laizi-x-xingqiu`
- Local default path: `/Users/xiangbaqiu/.openclaw/workspace-cto-agent/xiaohongshu-yunying-laizi-x-xingqiu`

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

## Notes

This repository intentionally stays lightweight.
It contains the **skill definition and operator references**, not the full implementation project.

## License

MIT
