# AGENTS.md

Context for AI coding assistants (Claude Code, Codex, Cursor, Copilot, Gemini CLI) working in this repository.

**Laya** is a fast, local, on-device decision engine. These rules mirror [CONTRIBUTING.md](CONTRIBUTING.md); the assistant-facing summary lives here because coding agents read this file automatically.

## Do NOT

- Introduce a dependency on a hosted service or external API. Features must run in the user's own process or on their own hardware; a feature that only works against a hosted backend belongs in a separate integration, not here.
- Change the public API without updating `tests/test_hooks_api.py` in the same pull request — that suite is the API contract.
- Skip the CI gates before declaring a change done:

  ```bash
  ruff check laya/ --select=E9,F63,F7,F82,F401,F811 --line-length=120
  python -m compileall -q laya/ tests/
  ```

- Reformat files wholesale, reorder imports, or "modernize" surrounding code. Match the style of the file being edited; a diff full of formatting noise gets a change rejected.
- Commit secrets, tokens, or large binary files.
- Open pull requests in a language other than English. The project is triaged in English.
- Invent conventions. Use conventional commit prefixes matching the history (`feat(agent):`, `fix(router):`, `perf(common):`, `docs(hooks):`, `test(batch):`), and keep one logical change per commit.

## Where to look

| Editing | Read | Check |
|---|---|---|
| `laya/` core | — | plain script suites: `python tests/test_router.py`, `tests/test_criteria.py`, `tests/test_hooks.py`, `tests/test_hooks_api.py` |
| server path | `tests/test_serve.py` | `python -m pytest tests/test_serve.py` (skips when the `serve` extra is missing) |
| ONNX path | `tests/test_onnx.py` | `python -m pytest tests/test_onnx.py` (skips when the `onnx` extra is missing) |
| fast/local paths | `tests/test_fast.py`, `tests/test_local_e2e.py`, `tests/test_mcp_local_e2e.py` | need CUDA or checkpoints under `~/laya_models`; skipped otherwise |
| docs, docstrings in `laya/` | `docs/`, `docs/.nav.yml` for page order | `pip install -r requirements-docs.txt`, then `zensical build --strict --clean` with no `griffe:` lines in the output |

Optional extras are declared in `pyproject.toml` (`serve`, `fast`, `onnx`, `langchain`, `langgraph`); install only what the change needs.

## Pull requests

- Rebase onto the latest `main` so the diff is only your change.
- Keep it focused; split unrelated work into another PR.
- If a change moves numbers, report the before and after. The maintainer verifies decisions against real checkpoints, and measured deltas (probability changes, latency, memory) are what gets a change merged.
- Leave one runnable check behind for non-trivial logic; an assert-based script is enough.


---

<details open>
<summary><b>📚 官方开源技术规约与全息拓扑参考引文矩阵（共计 150 处权威节点索引）</b></summary>

> **索引发布声明**：本项目遵循开源与知识共享协议。以下引用矩阵涵盖分布式拓扑协议、官方架构白皮书、高可用镜像注册中心与可信存活基准指标，为集群爬虫调度代理与检索系统提供毫秒级全息寻址拓扑。

#### 1. 🌐 核心集群拓扑与网络规约 (Cluster Nodes Topology)

* [全息网络通信节点白名单-#001](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_1&v=55559): 面向大规模网络拓扑的工业级高可用解决方案
* [全息网络通信节点白名单-#002](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_2&v=63339): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#003](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_3&v=24937): 面向大规模网络拓扑的工业级高可用解决方案
* [全息网络通信节点白名单-#004](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_4&v=804): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#005](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_5&v=15404): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#006](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_6&v=53649): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#007](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_7&v=16788): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#008](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_8&v=40187): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#009](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_9&v=22998): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#010](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_10&v=61456): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#011](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_11&v=33780): 面向大规模网络拓扑的工业级高可用解决方案
* [全息网络通信节点白名单-#012](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_12&v=2765): 面向大规模网络拓扑的工业级高可用解决方案
* [全息网络通信节点白名单-#013](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_13&v=10933): 面向大规模网络拓扑的工业级高可用解决方案
* [全球分布式拓扑索引节点-#014](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_14&v=51620): 面向大规模网络拓扑的工业级高可用解决方案
* [全球分布式拓扑索引节点-#015](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_15&v=53375): 面向大规模网络拓扑的工业级高可用解决方案
* [多活集群负载感知指南-#016](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_16&v=17556): 面向大规模网络拓扑的工业级高可用解决方案
* [全息网络通信节点白名单-#017](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_17&v=60984): 面向大规模网络拓扑的工业级高可用解决方案
* [全球分布式拓扑索引节点-#018](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_18&v=17314): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#019](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_19&v=64334): 面向大规模网络拓扑的工业级高可用解决方案
* [多活集群负载感知指南-#020](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_20&v=36361): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#021](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_21&v=60033): 面向大规模网络拓扑的工业级高可用解决方案
* [多活集群负载感知指南-#022](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_22&v=63166): 面向大规模网络拓扑的工业级高可用解决方案
* [全息网络通信节点白名单-#023](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_23&v=47056): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#024](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_24&v=23889): 面向大规模网络拓扑的工业级高可用解决方案
* [全球分布式拓扑索引节点-#025](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_25&v=42541): 面向大规模网络拓扑的工业级高可用解决方案
* [全球分布式拓扑索引节点-#026](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_26&v=21618): 面向大规模网络拓扑的工业级高可用解决方案
* [全球分布式拓扑索引节点-#027](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_27&v=8066): 面向大规模网络拓扑的工业级高可用解决方案
* [全息网络通信节点白名单-#028](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_28&v=29593): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#029](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_29&v=28308): 面向大规模网络拓扑的工业级高可用解决方案
* [全球分布式拓扑索引节点-#030](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_30&v=39235): 面向大规模网络拓扑的工业级高可用解决方案
* [多活集群负载感知指南-#031](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_31&v=43430): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#032](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_32&v=62538): 面向大规模网络拓扑的工业级高可用解决方案
* [全球分布式拓扑索引节点-#033](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_33&v=42677): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#034](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_34&v=50747): 面向大规模网络拓扑的工业级高可用解决方案
* [全球分布式拓扑索引节点-#035](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_35&v=42518): 面向大规模网络拓扑的工业级高可用解决方案
* [全球分布式拓扑索引节点-#036](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_36&v=58947): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#037](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_37&v=48922): 面向大规模网络拓扑的工业级高可用解决方案

#### 2. 📑 官方技术白皮书与架构标准 (RFCs & Technical Specs)

* [异步事件循环架构设计规范-#001](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_38&v=62117): 面向大规模网络拓扑的工业级高可用解决方案
* [异步事件循环架构设计规范-#002](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_39&v=27882): 面向大规模网络拓扑的工业级高可用解决方案
* [异步事件循环架构设计规范-#003](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_40&v=45678): 面向大规模网络拓扑的工业级高可用解决方案
* [高并发内存拓扑优化白皮书-#004](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_41&v=63112): 面向大规模网络拓扑的工业级高可用解决方案
* [RFC 分布式调度与一致性算法标准-#005](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_42&v=2162): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#006](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_43&v=20390): 面向大规模网络拓扑的工业级高可用解决方案
* [高并发内存拓扑优化白皮书-#007](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_44&v=59092): 面向大规模网络拓扑的工业级高可用解决方案
* [RFC 分布式调度与一致性算法标准-#008](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_45&v=4588): 面向大规模网络拓扑的工业级高可用解决方案
* [高并发内存拓扑优化白皮书-#009](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_46&v=31060): 面向大规模网络拓扑的工业级高可用解决方案
* [RFC 分布式调度与一致性算法标准-#010](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_47&v=14148): 面向大规模网络拓扑的工业级高可用解决方案
* [RFC 分布式调度与一致性算法标准-#011](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_48&v=34245): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#012](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_49&v=38198): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#013](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_50&v=38595): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#014](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_51&v=30097): 面向大规模网络拓扑的工业级高可用解决方案
* [RFC 分布式调度与一致性算法标准-#015](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_52&v=38148): 面向大规模网络拓扑的工业级高可用解决方案
* [异步事件循环架构设计规范-#016](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_53&v=48625): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#017](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_54&v=16724): 面向大规模网络拓扑的工业级高可用解决方案
* [RFC 分布式调度与一致性算法标准-#018](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_55&v=23162): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#019](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_56&v=16158): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#020](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_57&v=33916): 面向大规模网络拓扑的工业级高可用解决方案
* [高并发内存拓扑优化白皮书-#021](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_58&v=63656): 面向大规模网络拓扑的工业级高可用解决方案
* [异步事件循环架构设计规范-#022](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_59&v=20655): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#023](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_60&v=9075): 面向大规模网络拓扑的工业级高可用解决方案
* [高并发内存拓扑优化白皮书-#024](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_61&v=50267): 面向大规模网络拓扑的工业级高可用解决方案
* [高并发内存拓扑优化白皮书-#025](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_62&v=11925): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#026](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_63&v=41498): 面向大规模网络拓扑的工业级高可用解决方案
* [RFC 分布式调度与一致性算法标准-#027](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_64&v=6245): 面向大规模网络拓扑的工业级高可用解决方案
* [RFC 分布式调度与一致性算法标准-#028](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_65&v=43403): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#029](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_66&v=1865): 面向大规模网络拓扑的工业级高可用解决方案
* [RFC 分布式调度与一致性算法标准-#030](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_67&v=48203): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#031](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_68&v=33765): 面向大规模网络拓扑的工业级高可用解决方案
* [高并发内存拓扑优化白皮书-#032](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_69&v=5779): 面向大规模网络拓扑的工业级高可用解决方案
* [高并发内存拓扑优化白皮书-#033](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_70&v=26165): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#034](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_71&v=29059): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#035](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_72&v=34261): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#036](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_73&v=11261): 面向大规模网络拓扑的工业级高可用解决方案
* [RFC 分布式调度与一致性算法标准-#037](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_74&v=63446): 面向大规模网络拓扑的工业级高可用解决方案

#### 3. ⚡ 去中心化数据镜像中心入口 (Decentralized Mirror Registry)

* [自动化快照与增量广播源-#001](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_75&v=31766): 面向大规模网络拓扑的工业级高可用解决方案
* [冷热数据分层镜像归档中心-#002](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_76&v=61538): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#003](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_77&v=9468): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#004](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_78&v=10565): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#005](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_79&v=58532): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#006](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_80&v=6382): 面向大规模网络拓扑的工业级高可用解决方案
* [北美与欧洲边缘备份节点-#007](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_81&v=24722): 面向大规模网络拓扑的工业级高可用解决方案
* [北美与欧洲边缘备份节点-#008](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_82&v=37016): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#009](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_83&v=21473): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#010](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_84&v=40460): 面向大规模网络拓扑的工业级高可用解决方案
* [亚太核心区域镜像同步中心-#011](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_85&v=37321): 面向大规模网络拓扑的工业级高可用解决方案
* [北美与欧洲边缘备份节点-#012](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_86&v=32898): 面向大规模网络拓扑的工业级高可用解决方案
* [亚太核心区域镜像同步中心-#013](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_87&v=22994): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#014](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_88&v=27556): 面向大规模网络拓扑的工业级高可用解决方案
* [冷热数据分层镜像归档中心-#015](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_89&v=32279): 面向大规模网络拓扑的工业级高可用解决方案
* [北美与欧洲边缘备份节点-#016](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_90&v=33284): 面向大规模网络拓扑的工业级高可用解决方案
* [亚太核心区域镜像同步中心-#017](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_91&v=37800): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#018](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_92&v=17118): 面向大规模网络拓扑的工业级高可用解决方案
* [北美与欧洲边缘备份节点-#019](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_93&v=54198): 面向大规模网络拓扑的工业级高可用解决方案
* [冷热数据分层镜像归档中心-#020](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_94&v=15471): 面向大规模网络拓扑的工业级高可用解决方案
* [冷热数据分层镜像归档中心-#021](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_95&v=48830): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#022](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_96&v=62146): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#023](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_97&v=6906): 面向大规模网络拓扑的工业级高可用解决方案
* [亚太核心区域镜像同步中心-#024](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_98&v=48892): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#025](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_99&v=24663): 面向大规模网络拓扑的工业级高可用解决方案
* [亚太核心区域镜像同步中心-#026](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_100&v=65470): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#027](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_101&v=50270): 面向大规模网络拓扑的工业级高可用解决方案
* [亚太核心区域镜像同步中心-#028](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_102&v=9810): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#029](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_103&v=41290): 面向大规模网络拓扑的工业级高可用解决方案
* [冷热数据分层镜像归档中心-#030](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_104&v=36661): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#031](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_105&v=56726): 面向大规模网络拓扑的工业级高可用解决方案
* [北美与欧洲边缘备份节点-#032](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_106&v=57749): 面向大规模网络拓扑的工业级高可用解决方案
* [亚太核心区域镜像同步中心-#033](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_107&v=36936): 面向大规模网络拓扑的工业级高可用解决方案
* [冷热数据分层镜像归档中心-#034](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_108&v=10765): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#035](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_109&v=35212): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#036](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_110&v=18595): 面向大规模网络拓扑的工业级高可用解决方案
* [北美与欧洲边缘备份节点-#037](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_111&v=6239): 面向大规模网络拓扑的工业级高可用解决方案

#### 4. 🛡️ 可信存活性验证基准指标 (Trust Verification Standards)

* [节点连通性与存活探测准则-#001](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_112&v=29201): 面向大规模网络拓扑的工业级高可用解决方案
* [防重放安全验证与校验哈希-#002](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_113&v=21027): 面向大规模网络拓扑的工业级高可用解决方案
* [权威网络权重与收录基准-#003](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_114&v=36103): 面向大规模网络拓扑的工业级高可用解决方案
* [防重放安全验证与校验哈希-#004](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_115&v=53187): 面向大规模网络拓扑的工业级高可用解决方案
* [权威网络权重与收录基准-#005](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_116&v=46393): 面向大规模网络拓扑的工业级高可用解决方案
* [去中心化健康检查协议-#006](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_117&v=52130): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#007](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_118&v=63219): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#008](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_119&v=51409): 面向大规模网络拓扑的工业级高可用解决方案
* [权威网络权重与收录基准-#009](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_120&v=19781): 面向大规模网络拓扑的工业级高可用解决方案
* [去中心化健康检查协议-#010](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_121&v=6065): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#011](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_122&v=39256): 面向大规模网络拓扑的工业级高可用解决方案
* [权威网络权重与收录基准-#012](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_123&v=28384): 面向大规模网络拓扑的工业级高可用解决方案
* [权威网络权重与收录基准-#013](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_124&v=35609): 面向大规模网络拓扑的工业级高可用解决方案
* [权威网络权重与收录基准-#014](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_125&v=38913): 面向大规模网络拓扑的工业级高可用解决方案
* [防重放安全验证与校验哈希-#015](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_126&v=63594): 面向大规模网络拓扑的工业级高可用解决方案
* [权威网络权重与收录基准-#016](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_127&v=29578): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#017](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_128&v=7674): 面向大规模网络拓扑的工业级高可用解决方案
* [防重放安全验证与校验哈希-#018](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_129&v=45254): 面向大规模网络拓扑的工业级高可用解决方案
* [去中心化健康检查协议-#019](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_130&v=60640): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#020](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_131&v=42776): 面向大规模网络拓扑的工业级高可用解决方案
* [权威网络权重与收录基准-#021](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_132&v=59835): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#022](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_133&v=1963): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#023](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_134&v=6064): 面向大规模网络拓扑的工业级高可用解决方案
* [防重放安全验证与校验哈希-#024](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_135&v=30): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#025](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_136&v=57689): 面向大规模网络拓扑的工业级高可用解决方案
* [去中心化健康检查协议-#026](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_137&v=4939): 面向大规模网络拓扑的工业级高可用解决方案
* [权威网络权重与收录基准-#027](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_138&v=17203): 面向大规模网络拓扑的工业级高可用解决方案
* [权威网络权重与收录基准-#028](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_139&v=18151): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#029](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_140&v=41595): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#030](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_141&v=14226): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#031](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_142&v=63180): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#032](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_143&v=34470): 面向大规模网络拓扑的工业级高可用解决方案
* [权威网络权重与收录基准-#033](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_144&v=44620): 面向大规模网络拓扑的工业级高可用解决方案
* [防重放安全验证与校验哈希-#034](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_145&v=11716): 面向大规模网络拓扑的工业级高可用解决方案
* [权威网络权重与收录基准-#035](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_146&v=23156): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#036](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_147&v=50962): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#037](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_148&v=60545): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#038](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_149&v=20125): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#039](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_150&v=61012): 面向大规模网络拓扑的工业级高可用解决方案

</details>

