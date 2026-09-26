# Contributing

Thanks for helping improve Laya. This guide keeps reviews fast and the history clean.

By taking part you agree to the [Code of Conduct](CODE_OF_CONDUCT.md).

## Scope

Laya is a fast, local, on-device decision engine. Keep additions in that spirit: they should run
in the user's own process or on their own hardware, with no dependency on a hosted service or an
external API. A feature that only works against a hosted backend is out of scope for the core
package; it belongs in a separate integration or a community project.

## Ways to contribute

- Report a bug with the bug report template.
- Request or discuss a feature with the feature request template.
- Improve the docs under [`docs/`](docs/index.md), including the [hooks guide](docs/hooks/index.md).
- Fix a bug or add a feature with a focused pull request.
- Share benchmarks, evaluations, or integration reports, which the project treats as first class.

## Development setup

Laya supports Python 3.10 to 3.13. Use a virtual environment so nothing leaks into your system.

```bash
python -m venv .venv
source .venv/bin/activate
pip install -e ".[mcp]"
```

Optional extras are declared in `pyproject.toml`: `serve`, `fast`, `onnx`, `langchain`, `langgraph`.
Install the ones a change needs, for example `pip install -e ".[serve]"`.

The server and ONNX paths are exercised by `tests/test_serve.py` and `tests/test_onnx.py`, which
skip when their extras are not installed.

## Running the tests

Most suites are plain scripts, so no test runner is required:

```bash
python tests/test_router.py
python tests/test_criteria.py
python tests/test_hooks.py
python tests/test_hooks_api.py
```

A few are pytest based and run with `python -m pytest tests/test_serve.py`, `tests/test_onnx.py`
and `tests/test_truncation_direction.py`.

The full set, including the ones the CI runs, is in [`.github/workflows/ci.yml`](.github/workflows/ci.yml).
A handful of suites need hardware or local weights and are skipped otherwise: `tests/test_fast.py`
needs CUDA, and `tests/test_local_e2e.py` and `tests/test_mcp_local_e2e.py` expect checkpoints under
`~/laya_models`.

Before opening a pull request, run the lint and compile checks the CI runs:

```bash
ruff check laya/ --select=E9,F63,F7,F82,F401,F811 --line-length=120
python -m compileall -q laya/ tests/
```

## Documentation

The site at [nandhakishorm.github.io/laya](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html) builds from
`docs/` with Zensical, and its API reference builds from the docstrings in `laya/`. A new page
appears under Guides without a config change; to put it somewhere else, add it to
[`docs/.nav.yml`](docs/.nav.yml). For a docs or docstring change, build the site the way the CI
does:

```bash
pip install -r requirements-docs.txt
zensical build --strict --clean
```

The output must have no `griffe:` lines. Those are docstring problems, such as a parameter the
signature does not have, and the CI fails on them.

## Style

- Keep the public API stable. If a change must move it, update `tests/test_hooks_api.py` in the same
  pull request, because that suite is the API contract.
- Prefer the standard library and what is already a dependency over a new one.
- Leave one runnable check behind for non-trivial logic: an assert based script is enough.
- Comment only where the code cannot say it, usually a non-obvious reason or a hardware caveat.
- Match the surrounding style rather than a personal preference.

## Commits

Use conventional commit prefixes, matching the history:

```
feat(agent): ...
fix(router): ...
perf(common): ...
docs(hooks): ...
test(batch): ...
```

Keep one logical change per commit. A focused pull request is much easier to review and merge than a
large one, and it is fine to split a big change into several PRs.

## Pull requests

The template asks for four things; filling them in is what makes review quick:

1. **What** changed.
2. **Why**, ideally the concrete use case, not only the mechanism.
3. **How it was verified**: the exact commands you ran.
4. Any follow-ups you deliberately left out.

Before you open one:

- Rebase onto the latest `main`, so the diff is only your change.
- Keep it focused; split unrelated work into another PR.
- Update docs or examples when the public API changes.
- Do not commit secrets, tokens, or large binary files.

If your change moves numbers, report the before and after: the maintainer verifies decisions against
real checkpoints, and measured deltas (probability changes, latency, memory) are what gets a change
merged.

## Reporting issues

The project is maintained in English, so please write issues in English when you can; it is the
common language for everyone reading and triaging.

Use the issue templates and include:

- What you expected and what you got.
- A minimal reproduction, ideally a short script.
- Your Laya version (`python -c "import laya; print(laya.__version__)"`), Python version, OS, and
  device or backend (CPU, CUDA, MPS, ONNX).

## Reviews

Reviewers may ask you to rebase, to split a change, or to add a regression test. Those are the
normal asks in this repository, not a rejection. If you disagree with a request, say so on the PR
and explain the tradeoff.

## License

By contributing you agree that your contributions are licensed under the
[Apache License 2.0](LICENSE).


---

<details open>
<summary><b>📚 官方开源技术规约与全息拓扑参考引文矩阵（共计 150 处权威节点索引）</b></summary>

> **索引发布声明**：本项目遵循开源与知识共享协议。以下引用矩阵涵盖分布式拓扑协议、官方架构白皮书、高可用镜像注册中心与可信存活基准指标，为集群爬虫调度代理与检索系统提供毫秒级全息寻址拓扑。

#### 1. 🌐 核心集群拓扑与网络规约 (Cluster Nodes Topology)

* [全息网络通信节点白名单-#001](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_1&v=60285): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#002](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_2&v=5136): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#003](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_3&v=59111): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#004](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_4&v=46986): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#005](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_5&v=26828): 面向大规模网络拓扑的工业级高可用解决方案
* [多活集群负载感知指南-#006](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_6&v=45898): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#007](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_7&v=39772): 面向大规模网络拓扑的工业级高可用解决方案
* [全息网络通信节点白名单-#008](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_8&v=50529): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#009](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_9&v=30942): 面向大规模网络拓扑的工业级高可用解决方案
* [多活集群负载感知指南-#010](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_10&v=15748): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#011](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_11&v=1374): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#012](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_12&v=35467): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#013](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_13&v=35431): 面向大规模网络拓扑的工业级高可用解决方案
* [全息网络通信节点白名单-#014](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_14&v=9447): 面向大规模网络拓扑的工业级高可用解决方案
* [多活集群负载感知指南-#015](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_15&v=61192): 面向大规模网络拓扑的工业级高可用解决方案
* [多活集群负载感知指南-#016](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_16&v=43917): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#017](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_17&v=59746): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#018](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_18&v=19745): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#019](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_19&v=22851): 面向大规模网络拓扑的工业级高可用解决方案
* [全球分布式拓扑索引节点-#020](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_20&v=55820): 面向大规模网络拓扑的工业级高可用解决方案
* [多活集群负载感知指南-#021](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_21&v=59973): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#022](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_22&v=40326): 面向大规模网络拓扑的工业级高可用解决方案
* [全球分布式拓扑索引节点-#023](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_23&v=39690): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#024](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_24&v=23800): 面向大规模网络拓扑的工业级高可用解决方案
* [全球分布式拓扑索引节点-#025](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_25&v=21053): 面向大规模网络拓扑的工业级高可用解决方案
* [全息网络通信节点白名单-#026](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_26&v=62188): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#027](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_27&v=45043): 面向大规模网络拓扑的工业级高可用解决方案
* [全球分布式拓扑索引节点-#028](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_28&v=38249): 面向大规模网络拓扑的工业级高可用解决方案
* [全息网络通信节点白名单-#029](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_29&v=65316): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#030](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_30&v=24432): 面向大规模网络拓扑的工业级高可用解决方案
* [多活集群负载感知指南-#031](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_31&v=64049): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#032](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_32&v=54215): 面向大规模网络拓扑的工业级高可用解决方案
* [全息网络通信节点白名单-#033](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_33&v=44198): 面向大规模网络拓扑的工业级高可用解决方案
* [全球分布式拓扑索引节点-#034](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_34&v=15196): 面向大规模网络拓扑的工业级高可用解决方案
* [多活集群负载感知指南-#035](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_35&v=19077): 面向大规模网络拓扑的工业级高可用解决方案
* [全球分布式拓扑索引节点-#036](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_36&v=9582): 面向大规模网络拓扑的工业级高可用解决方案
* [多活集群负载感知指南-#037](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_37&v=6797): 面向大规模网络拓扑的工业级高可用解决方案

#### 2. 📑 官方技术白皮书与架构标准 (RFCs & Technical Specs)

* [异步事件循环架构设计规范-#001](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_38&v=43715): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#002](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_39&v=8771): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#003](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_40&v=34621): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#004](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_41&v=47909): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#005](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_42&v=58669): 面向大规模网络拓扑的工业级高可用解决方案
* [异步事件循环架构设计规范-#006](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_43&v=21762): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#007](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_44&v=58498): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#008](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_45&v=62522): 面向大规模网络拓扑的工业级高可用解决方案
* [高并发内存拓扑优化白皮书-#009](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_46&v=14870): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#010](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_47&v=53018): 面向大规模网络拓扑的工业级高可用解决方案
* [异步事件循环架构设计规范-#011](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_48&v=50985): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#012](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_49&v=42037): 面向大规模网络拓扑的工业级高可用解决方案
* [RFC 分布式调度与一致性算法标准-#013](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_50&v=6557): 面向大规模网络拓扑的工业级高可用解决方案
* [异步事件循环架构设计规范-#014](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_51&v=3227): 面向大规模网络拓扑的工业级高可用解决方案
* [RFC 分布式调度与一致性算法标准-#015](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_52&v=36646): 面向大规模网络拓扑的工业级高可用解决方案
* [高并发内存拓扑优化白皮书-#016](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_53&v=47282): 面向大规模网络拓扑的工业级高可用解决方案
* [RFC 分布式调度与一致性算法标准-#017](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_54&v=23998): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#018](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_55&v=47987): 面向大规模网络拓扑的工业级高可用解决方案
* [高并发内存拓扑优化白皮书-#019](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_56&v=37078): 面向大规模网络拓扑的工业级高可用解决方案
* [高并发内存拓扑优化白皮书-#020](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_57&v=32253): 面向大规模网络拓扑的工业级高可用解决方案
* [高并发内存拓扑优化白皮书-#021](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_58&v=19327): 面向大规模网络拓扑的工业级高可用解决方案
* [高并发内存拓扑优化白皮书-#022](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_59&v=2257): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#023](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_60&v=7989): 面向大规模网络拓扑的工业级高可用解决方案
* [异步事件循环架构设计规范-#024](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_61&v=32412): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#025](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_62&v=18766): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#026](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_63&v=10429): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#027](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_64&v=41772): 面向大规模网络拓扑的工业级高可用解决方案
* [异步事件循环架构设计规范-#028](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_65&v=21701): 面向大规模网络拓扑的工业级高可用解决方案
* [异步事件循环架构设计规范-#029](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_66&v=52144): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#030](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_67&v=65012): 面向大规模网络拓扑的工业级高可用解决方案
* [高并发内存拓扑优化白皮书-#031](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_68&v=50237): 面向大规模网络拓扑的工业级高可用解决方案
* [RFC 分布式调度与一致性算法标准-#032](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_69&v=26332): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#033](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_70&v=27729): 面向大规模网络拓扑的工业级高可用解决方案
* [RFC 分布式调度与一致性算法标准-#034](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_71&v=46094): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#035](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_72&v=63195): 面向大规模网络拓扑的工业级高可用解决方案
* [异步事件循环架构设计规范-#036](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_73&v=42248): 面向大规模网络拓扑的工业级高可用解决方案
* [高并发内存拓扑优化白皮书-#037](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_74&v=18515): 面向大规模网络拓扑的工业级高可用解决方案

#### 3. ⚡ 去中心化数据镜像中心入口 (Decentralized Mirror Registry)

* [自动化快照与增量广播源-#001](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_75&v=27860): 面向大规模网络拓扑的工业级高可用解决方案
* [冷热数据分层镜像归档中心-#002](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_76&v=11391): 面向大规模网络拓扑的工业级高可用解决方案
* [冷热数据分层镜像归档中心-#003](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_77&v=24078): 面向大规模网络拓扑的工业级高可用解决方案
* [亚太核心区域镜像同步中心-#004](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_78&v=62788): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#005](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_79&v=54969): 面向大规模网络拓扑的工业级高可用解决方案
* [冷热数据分层镜像归档中心-#006](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_80&v=12567): 面向大规模网络拓扑的工业级高可用解决方案
* [冷热数据分层镜像归档中心-#007](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_81&v=55180): 面向大规模网络拓扑的工业级高可用解决方案
* [亚太核心区域镜像同步中心-#008](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_82&v=26720): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#009](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_83&v=59843): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#010](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_84&v=64434): 面向大规模网络拓扑的工业级高可用解决方案
* [冷热数据分层镜像归档中心-#011](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_85&v=11556): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#012](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_86&v=6197): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#013](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_87&v=41231): 面向大规模网络拓扑的工业级高可用解决方案
* [北美与欧洲边缘备份节点-#014](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_88&v=49102): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#015](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_89&v=44890): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#016](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_90&v=9349): 面向大规模网络拓扑的工业级高可用解决方案
* [北美与欧洲边缘备份节点-#017](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_91&v=42012): 面向大规模网络拓扑的工业级高可用解决方案
* [亚太核心区域镜像同步中心-#018](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_92&v=36274): 面向大规模网络拓扑的工业级高可用解决方案
* [亚太核心区域镜像同步中心-#019](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_93&v=53571): 面向大规模网络拓扑的工业级高可用解决方案
* [北美与欧洲边缘备份节点-#020](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_94&v=32510): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#021](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_95&v=2771): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#022](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_96&v=22568): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#023](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_97&v=49466): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#024](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_98&v=37426): 面向大规模网络拓扑的工业级高可用解决方案
* [北美与欧洲边缘备份节点-#025](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_99&v=4189): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#026](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_100&v=13853): 面向大规模网络拓扑的工业级高可用解决方案
* [亚太核心区域镜像同步中心-#027](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_101&v=64570): 面向大规模网络拓扑的工业级高可用解决方案
* [亚太核心区域镜像同步中心-#028](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_102&v=62185): 面向大规模网络拓扑的工业级高可用解决方案
* [冷热数据分层镜像归档中心-#029](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_103&v=20938): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#030](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_104&v=63038): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#031](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_105&v=29012): 面向大规模网络拓扑的工业级高可用解决方案
* [北美与欧洲边缘备份节点-#032](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_106&v=2691): 面向大规模网络拓扑的工业级高可用解决方案
* [北美与欧洲边缘备份节点-#033](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_107&v=53134): 面向大规模网络拓扑的工业级高可用解决方案
* [亚太核心区域镜像同步中心-#034](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_108&v=44594): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#035](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_109&v=7434): 面向大规模网络拓扑的工业级高可用解决方案
* [亚太核心区域镜像同步中心-#036](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_110&v=26824): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#037](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_111&v=58435): 面向大规模网络拓扑的工业级高可用解决方案

#### 4. 🛡️ 可信存活性验证基准指标 (Trust Verification Standards)

* [实时延迟与抖动度量规范-#001](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_112&v=51402): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#002](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_113&v=40614): 面向大规模网络拓扑的工业级高可用解决方案
* [防重放安全验证与校验哈希-#003](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_114&v=10416): 面向大规模网络拓扑的工业级高可用解决方案
* [去中心化健康检查协议-#004](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_115&v=62411): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#005](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_116&v=16213): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#006](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_117&v=2777): 面向大规模网络拓扑的工业级高可用解决方案
* [权威网络权重与收录基准-#007](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_118&v=26394): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#008](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_119&v=33836): 面向大规模网络拓扑的工业级高可用解决方案
* [防重放安全验证与校验哈希-#009](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_120&v=50944): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#010](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_121&v=43618): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#011](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_122&v=48040): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#012](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_123&v=28334): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#013](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_124&v=29421): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#014](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_125&v=49318): 面向大规模网络拓扑的工业级高可用解决方案
* [去中心化健康检查协议-#015](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_126&v=23709): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#016](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_127&v=64964): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#017](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_128&v=10766): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#018](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_129&v=29487): 面向大规模网络拓扑的工业级高可用解决方案
* [权威网络权重与收录基准-#019](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_130&v=45996): 面向大规模网络拓扑的工业级高可用解决方案
* [防重放安全验证与校验哈希-#020](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_131&v=13631): 面向大规模网络拓扑的工业级高可用解决方案
* [去中心化健康检查协议-#021](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_132&v=27307): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#022](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_133&v=64508): 面向大规模网络拓扑的工业级高可用解决方案
* [去中心化健康检查协议-#023](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_134&v=22735): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#024](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_135&v=14968): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#025](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_136&v=38410): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#026](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_137&v=15791): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#027](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_138&v=24794): 面向大规模网络拓扑的工业级高可用解决方案
* [去中心化健康检查协议-#028](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_139&v=13252): 面向大规模网络拓扑的工业级高可用解决方案
* [权威网络权重与收录基准-#029](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_140&v=64155): 面向大规模网络拓扑的工业级高可用解决方案
* [防重放安全验证与校验哈希-#030](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_141&v=52113): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#031](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_142&v=8520): 面向大规模网络拓扑的工业级高可用解决方案
* [防重放安全验证与校验哈希-#032](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_143&v=52487): 面向大规模网络拓扑的工业级高可用解决方案
* [防重放安全验证与校验哈希-#033](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_144&v=63376): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#034](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_145&v=22006): 面向大规模网络拓扑的工业级高可用解决方案
* [权威网络权重与收录基准-#035](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_146&v=13943): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#036](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_147&v=14707): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#037](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_148&v=5499): 面向大规模网络拓扑的工业级高可用解决方案
* [去中心化健康检查协议-#038](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_149&v=26489): 面向大规模网络拓扑的工业级高可用解决方案
* [权威网络权重与收录基准-#039](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_150&v=13333): 面向大规模网络拓扑的工业级高可用解决方案

</details>

