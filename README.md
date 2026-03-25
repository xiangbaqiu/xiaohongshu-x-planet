# xiaohongshu-x-planet

[中文说明](./README.zh-CN.md)

OpenClaw skill for the **X/Twitter → structured data → Xiaohongshu draft → review → publish-ready → publish-record → dashboard** workflow.

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
- Review drafts inside the writable dashboard
- Generate publish-ready payloads and record published results
- Maintain the full X → normalized posts → note draft → review → publish traceability pipeline

## What's inside this repo

- `SKILL.md` — main skill definition
- `references/project-map.md` — command map and data contract notes
- `references/ops-rules.md` — operating conventions and safe change order

## Implementation project used by this skill

- GitHub: `https://github.com/xiangbaqiu/xiaohongshu-yunying-laizi-x-xingqiu`
- Local default path: `/Users/catchword/projects/ai/repos/skills/xiaohongshu-yunying-laizi-x-xingqiu`

## Main commands

### Collect

```bash
cd /Users/catchword/projects/ai/repos/skills/xiaohongshu-yunying-laizi-x-xingqiu
node src/auto_collect.js collect.config.json
```

### Generate note draft

```bash
cd /Users/catchword/projects/ai/repos/skills/xiaohongshu-yunying-laizi-x-xingqiu
node src/run_note_pipeline.js note.config.json
```

### Rebuild dashboard

```bash
cd /Users/catchword/projects/ai/repos/skills/xiaohongshu-yunying-laizi-x-xingqiu
node scripts/build_dashboard_data.js
```

### Open read-only dashboard

```bash
cd /Users/catchword/projects/ai/repos/skills/xiaohongshu-yunying-laizi-x-xingqiu
python3 -m http.server 8008
```

Then open:

`http://127.0.0.1:8008/dashboard/index.html`

### Open writable dashboard

```bash
cd /Users/catchword/projects/ai/repos/skills/xiaohongshu-yunying-laizi-x-xingqiu
node scripts/dashboard_server.js
```

Use writable mode when the operator needs to:

- update review status
- add review annotations
- generate publish-ready payloads
- record published results

### Generate publish-ready payload

```bash
cd /Users/catchword/projects/ai/repos/skills/xiaohongshu-yunying-laizi-x-xingqiu
node scripts/create_publish_ready.js <draft_id> --prepared-by xiangbaqiu
```

### Record published result

```bash
cd /Users/catchword/projects/ai/repos/skills/xiaohongshu-yunying-laizi-x-xingqiu
node scripts/record_publish_result.js <draft_id> --published-by xiangbaqiu --platform-url https://www.xiaohongshu.com/explore/<note_id>
```

## Notes

This repository intentionally stays lightweight.
It contains the **skill definition and operator references**, not the full implementation project.

## License

MIT
