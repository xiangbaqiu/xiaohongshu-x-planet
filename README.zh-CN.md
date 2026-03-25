# xiaohongshu-x-planet

用于运行 **X/Twitter → 结构化数据 → 小红书草稿 → review → publish-ready → publish-record → dashboard** 工作流的 OpenClaw Skill。

这个仓库是 **skill / runbook 层**。
它定义了 agent 应该如何操作这条工作流、如何校验结果，以及如何与实现项目协同工作。

## 仓库关系

- **Skill 仓库：** `https://github.com/xiangbaqiu/xiaohongshu-x-planet`
- **实现仓库：** `https://github.com/xiangbaqiu/xiaohongshu-yunying-laizi-x-xingqiu`

## 这个 skill 覆盖什么

- 采集一个或多个 X 账号
- 按 `replace_latest` 模式保留每个账号最新 N 条内容
- 重建或检查本地运营 dashboard
- 基于多条 X 内容生成一篇小红书草稿
- 在可写 dashboard 中做审核和批注
- 生成 publish-ready payload，并记录 published 结果
- 维护整条 X → 标准化内容 → 草稿 → 审核 → 发布追踪 的工作流

## 仓库内容

- `SKILL.md`：主技能定义
- `references/project-map.md`：命令入口、目录和数据结构说明
- `references/ops-rules.md`：操作规则和安全变更顺序

## 这个 skill 依赖的实现项目

- GitHub：`https://github.com/xiangbaqiu/xiaohongshu-yunying-laizi-x-xingqiu`
- 本地默认路径：`/Users/catchword/projects/ai/repos/skills/xiaohongshu-yunying-laizi-x-xingqiu`

## 主要命令

### 采集账号内容

```bash
cd /Users/catchword/projects/ai/repos/skills/xiaohongshu-yunying-laizi-x-xingqiu
node src/auto_collect.js collect.config.json
```

### 生成小红书草稿

```bash
cd /Users/catchword/projects/ai/repos/skills/xiaohongshu-yunying-laizi-x-xingqiu
node src/run_note_pipeline.js note.config.json
```

### 重建 dashboard 数据

```bash
cd /Users/catchword/projects/ai/repos/skills/xiaohongshu-yunying-laizi-x-xingqiu
node scripts/build_dashboard_data.js
```

### 打开只读 dashboard

```bash
cd /Users/catchword/projects/ai/repos/skills/xiaohongshu-yunying-laizi-x-xingqiu
python3 -m http.server 8008
```

然后访问：

`http://127.0.0.1:8008/dashboard/index.html`

### 打开可写 dashboard

```bash
cd /Users/catchword/projects/ai/repos/skills/xiaohongshu-yunying-laizi-x-xingqiu
node scripts/dashboard_server.js
```

可写模式适合做这些事：

- 更新审核状态
- 记录审核批注
- 生成 publish-ready payload
- 记录真实发布结果

### 生成 publish-ready payload

```bash
cd /Users/catchword/projects/ai/repos/skills/xiaohongshu-yunying-laizi-x-xingqiu
node scripts/create_publish_ready.js <draft_id> --prepared-by xiangbaqiu
```

### 记录 published 结果

```bash
cd /Users/catchword/projects/ai/repos/skills/xiaohongshu-yunying-laizi-x-xingqiu
node scripts/record_publish_result.js <draft_id> --published-by xiangbaqiu --platform-url https://www.xiaohongshu.com/explore/<note_id>
```

## 说明

这个仓库刻意保持轻量。
它包含的是 **技能定义和操作说明**，不包含完整实现项目。

## License

MIT
