# Operations rules

## Preferred operator behavior

- Prefer `replace_latest` mode for routine monitoring
- Keep dashboard defaulted to original content view
- Treat repost/quote/reply as useful context, but not the default selection pool
- Preserve raw batches so normalization can be re-run after rule changes
- Generate note drafts from a post bundle, not from a single-post rewrite
- Show generated drafts in dashboard when operators need review in one place

## Safe change order

When evolving the system, prefer this order:

1. Adjust extraction or normalization
2. Rebuild account data from raw or rerun collector
3. Adjust note selection / brief / composition if needed
4. Rebuild dashboard JSON
5. Open dashboard for visual verification
6. Commit changes

## Common tasks

### Add monitored accounts

Edit `collect.config.json` and rerun auto collector.

### Increase quantity per account

Increase `count_per_account` and rerun with `replace_latest`.

### Keep only fresh recent sets

Use `mode: "replace_latest"`.

### Generate a new note draft

Edit `note.config.json` and run:

```bash
node src/run_note_pipeline.js note.config.json
```

### Inspect whether counts are correct

Check:

```bash
wc -l data/x/accounts/*/posts.jsonl
```

### Inspect account state

Check:

```bash
cat data/x/accounts/<handle>/state.json
```

### Inspect generated drafts

Check:

```bash
ls -1 notes/drafts
```

or open dashboard and inspect the `生成内容` panel.

## Non-goals for this skill

This skill is not for:

- building a hosted SaaS backend
- X comment/thread deep crawling
- media asset downloading pipeline
- sentiment/topic ML enrichment
- direct auto-publishing to Xiaohongshu

Those can be layered later, but are outside this skill's current scope.
