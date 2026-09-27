# manyi 实验公开数据档案

下载入口：[https://github.com/Siyuexi/manyi-experiment-archive/releases/tag/snapshot-2026-09-27](https://github.com/Siyuexi/manyi-experiment-archive/releases/tag/snapshot-2026-09-27)

这是 TokenAna 实验在 2026-09-27 停止后的**脱敏、按文件内容去重副本**。实验没有因本次归档重新启动。

范围覆盖 `runs/` 中 22 个实验/探测目录：主实验、已排除的 Codex 非 GPT 历史组合、失败/跳过/中断记录、准备与缓存修复 smoke。RQ1 目录目前只有配置/脚本，没有独立运行数据。

## 下载

请下载本 Release 中的全部 `manyi-data-20260927-objects-*.zip`，以及 `manyi-data-20260927-index.zip`、`restore.py`、`verify.py` 和 `SHA256SUMS`。无需 Git LFS，也不需要克隆源码仓库。

对象包：
- `manyi-data-20260927-objects-001.zip`
- `manyi-data-20260927-objects-002.zip`
- `manyi-data-20260927-objects-003.zip`

索引包包含清单、SHA-256 对象映射、范围说明和省略记录。对象 ZIP 内是按 SHA-256 命名的内容，不是直接可浏览的原目录；使用恢复工具后得到可浏览目录。

## 校验与恢复

需要 Python 3.9 或更新版本，仅使用标准库：

```bash
python verify.py .
python restore.py --archives . --output restored
```

恢复单个实验：

```bash
python restore.py --archives . --output selected --prefix runs/deepswe-api-35/
```

只查看索引：

```bash
python restore.py --archives . --list
```

需要节省还原后的磁盘，可加 `--hardlink`；相同内容的路径会共享文件，修改其中一个会影响其他硬链接。默认复制，不共享修改。输出目录必须为空，工具拒绝覆盖已有内容和目录穿越。

## 保留的数据

- 最终 session、原生 trajectory/rollout、框架日志、运行与失败记录。
- 原始 API request/response usage 证据，以及客户端前后版本；这些公开副本已脱敏。
- 实验配置快照、补丁、评测日志/结果、逐题结果和精简统计表。
- 仅导出 CLI 数据库中的会话/日志表为 JSONL，文件名追加 `.public.jsonl`；不发布认证表或 SQLite 缓存。

文件内容去重后，440,244 个逻辑路径映射到 351,663 个唯一对象。相同内容即使位于不同实验目录也只保存一次，路径关系仍完整记录在 `manifest.jsonl` 中。独立 HTTP 调用不因提示相似而合并；只是相同文件内容共用对象。

## 脱敏与省略范围

这是公开副本，**不是原始目录逐字节备份**。凭据、私钥、Bearer/JWT、内部服务地址、邮箱、运行身份和个人 home 路径经过脱敏；具体类型与次数在索引和 `ARCHIVE.json` 中。原本为 gzip 的响应先解码脱敏，恢复时按索引重新压缩，gzip 字节可能变化，但解码内容与归档校验值一致。

没有纳入旧 ZIP 导出、CLI 缓存/锁/数据库 sidecar、工作区与运行环境、逐调用算术计算树、重复的渲染报表和根目录拼接表；逐题结果和原始调用证据保留。详见 `EXCLUSIONS.json`，这些省略不是内容去重。原本已清理/未保存的文件无法补回。用户当前源码仓库保持不变，未公开其 Git 历史。

## 结果解释

`completed` 表示框架正常执行完成，不等于测试通过；`resolved=true` 才表示已有评测通过记录。失败、限流跳过和中断记录也是档案的一部分。停止时未收尾的任务可能没有最终 result.json，但已回收的 session/日志仍被纳入。

旧统计报告没有在此次导出时重新核算；历史缓存缺字段和费用不完整维持原记录。价表折算不等于 AIHub 实扣账单。不要把此档案的数量、缓存验证结果或部分评测当作完整有效实验。

日志可能包含第三方项目代码/测试片段，其原有许可继续适用；本数据发布不另行授予第三方内容权利。

## Archive metadata

- Snapshot started: 2026-09-27T13:55:27.027430+08:00
- Snapshot finished: 2026-09-27T14:19:43.198186+08:00
- Logical files: 440,244
- Unique objects: 351,663
- Unique uncompressed bytes: 19,864,592,959
- Compressed object bytes: 4,111,346,019
- Original experiment data modified: false
