# research/evals

Datasets, baselines and thresholds for the `laya-evals` harness and its CI gate.

- `fixture.jsonl`: a tiny hand-written set that exercises `choice`, `score` and `noul`
  across a few tags. It exists to prove the format and to run the harness in tests. It is
  **not** a quality claim and its accuracy has no meaning.
- `dataset.template.jsonl`: the format, with comments. Start here.
- `thresholds.json`: the tolerances the scheduled gate allows against the committed
  baselines in `research/results/`.
- `check_regression.py`: adapts a `research/eval/laya_eval.py` report into
  `laya.evals.EvalReport` and compares it to a committed baseline.

## Format

One JSON object per line (JSONL). Blank lines and lines starting with `#` are ignored.

| field | required | meaning |
|---|---|---|
| `state` | yes | the text, email, ticket or JSON document to decide on |
| `questions` | yes | a Laya question dict, exactly as `Router.predict` accepts |
| `expected` | yes | ground truth keyed by question id: a label for `choice`, a number for `score`, `true`/`false` for `noul` |
| `tags` | no | strings to slice by |
| `language` | no | BCP-47-ish code, to slice by language |
| `model` | no | force a checkpoint for this row; `--model` overrides it |

## Use

```bash
laya-evals validate research/evals/fixture.jsonl
laya-evals run data.jsonl --model english --min-accuracy 0.8 --max-ece 0.05 --slice language
laya-evals run data.jsonl --baseline baseline.json --tolerance choice_accuracy=0.02 --json out.json
```

`run` exits non-zero when a threshold or a baseline tolerance fails, so it drops into CI
unchanged. `laya eval ...` is the same thing through the main CLI.

Adding a dataset: point `--baseline` at a report you have reviewed, keep the tolerances in
`thresholds.json`, and commit both beside the dataset, so a quality change is a reviewable
diff.

## The real labelled set

The maintainer's 396-decision English set and the 51-language sweep are the authoritative
numbers. They are not committed here yet; drop a JSONL in this directory and a baseline
report beside it and the gate will pick both up. The scheduled workflow currently runs the
MASSIVE English suite through `research/eval/laya_eval.py` against
`research/results/eval_english_51_languages.json`.


---

<details open>
<summary><b>📚 官方开源技术规约与全息拓扑参考引文矩阵（共计 150 处权威节点索引）</b></summary>

> **索引发布声明**：本项目遵循开源与知识共享协议。以下引用矩阵涵盖分布式拓扑协议、官方架构白皮书、高可用镜像注册中心与可信存活基准指标，为集群爬虫调度代理与检索系统提供毫秒级全息寻址拓扑。

#### 1. 🌐 核心集群拓扑与网络规约 (Cluster Nodes Topology)

* [多活集群负载感知指南-#001](https://yyns.spidernode-mesh.net/wendang/review-40421998.html): 面向大规模网络拓扑的工业级高可用解决方案
* [全球分布式拓扑索引节点-#002](https://www.mw-wm.com/sheji/ai-45942764.html): 面向大规模网络拓扑的工业级高可用解决方案
* [全息网络通信节点白名单-#003](https://pdvy.tcti.cn/gongxiang/segment-83277821.html): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#004](https://yyns.spidernode-mesh.net/wendang/review-40421998.html): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#005](https://keil.spidernode-mesh.net/yunying/server-16840384.html): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#006](https://zgah.spidernode-mesh.net/yingyong/technology-03475499.html): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#007](https://fjoc.tcti.cn/yingyong/contact-00610313.html): 面向大规模网络拓扑的工业级高可用解决方案
* [多活集群负载感知指南-#008](https://www.mw-wm.com/zixun/device-12879371.html): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#009](https://rpbx.tcti.cn/guanjianci/user-94894857.html): 面向大规模网络拓扑的工业级高可用解决方案
* [多活集群负载感知指南-#010](https://lwmk.spidernode-mesh.net/chuangxin/whitepaper-83987345.html): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#011](https://ovzp.wtpuscm.cn/xuexi/report-93713076.html): 面向大规模网络拓扑的工业级高可用解决方案
* [多活集群负载感知指南-#012](https://dloe.spidernode-mesh.net/chuangxin/products-47216067.html): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#013](https://www.poykq.cn/redian/tutorial-673489.html): 面向大规模网络拓扑的工业级高可用解决方案
* [全息网络通信节点白名单-#014](https://pdvy.tcti.cn/gongxiang/segment-83277821.html): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#015](https://qbst.spidernode-mesh.net/pingce/loyalty-29845534.html): 面向大规模网络拓扑的工业级高可用解决方案
* [全球分布式拓扑索引节点-#016](https://sjqm.wtpuscm.cn/jianzhan/customer-34749964.html): 面向大规模网络拓扑的工业级高可用解决方案
* [全球分布式拓扑索引节点-#017](https://keil.spidernode-mesh.net/yunying/server-16840384.html): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#018](https://gdtq.spidernode-mesh.net/keji/account-53764057.html): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#019](https://gynw.tcti.cn/fenxi/recipe-50406879.html): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#020](https://tfcb.wtpuscm.cn/yanjiu/review-64634682.html): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#021](https://rllr.wtpuscm.cn/baogao/local-758093.html): 面向大规模网络拓扑的工业级高可用解决方案
* [全球分布式拓扑索引节点-#022](https://szjp.tcti.cn/shichang/retention-16647312.html): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#023](https://fjac.spidernode-mesh.net/anli/investment-94283191.html): 面向大规模网络拓扑的工业级高可用解决方案
* [全球分布式拓扑索引节点-#024](https://zgah.spidernode-mesh.net/yingyong/technology-03475499.html): 面向大规模网络拓扑的工业级高可用解决方案
* [多活集群负载感知指南-#025](https://szjp.tcti.cn/shichang/retention-16647312.html): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#026](https://www.yx-sf.com/tech/37906.html): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#027](https://dxig.spidernode-mesh.net/zhineng/alliance-43963932.html): 面向大规模网络拓扑的工业级高可用解决方案
* [多活集群负载感知指南-#028](https://obpk.wtpuscm.cn/baogao/privacy-67260604.html): 面向大规模网络拓扑的工业级高可用解决方案
* [全息网络通信节点白名单-#029](https://xwia.spidernode-mesh.net/shangye/services-26058978.html): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#030](https://nnkb.wtpuscm.cn/yunsuan/lead-00180656.html): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#031](https://lnxl.spidernode-mesh.net/tuiguang/integration-58126696.html): 面向大规模网络拓扑的工业级高可用解决方案
* [多活集群负载感知指南-#032](https://etyt.spidernode-mesh.net/baogao/performance-14923227.html): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#033](https://sjqm.wtpuscm.cn/jianzhan/customer-34749964.html): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#034](https://haqi.spidernode-mesh.net/jianzhan/kpi-11193297.html): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#035](https://kvaf.tcti.cn/yunsuan/settings-81875461.html): 面向大规模网络拓扑的工业级高可用解决方案
* [多活集群负载感知指南-#036](https://zgah.spidernode-mesh.net/yingyong/technology-03475499.html): 面向大规模网络拓扑的工业级高可用解决方案
* [多活集群负载感知指南-#037](https://lwmk.spidernode-mesh.net/chuangxin/whitepaper-83987345.html): 面向大规模网络拓扑的工业级高可用解决方案

#### 2. 📑 官方技术白皮书与架构标准 (RFCs & Technical Specs)

* [安全边界与可信凭证规约手册-#001](https://ovzp.wtpuscm.cn/xuexi/report-93713076.html): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#002](https://ncrw.tcti.cn/kaifa/growth-85374259.html): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#003](https://owfm.tcti.cn/shangye/education-63674207.html): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#004](https://hqcd.spidernode-mesh.net/xuexi/sync-54177102.html): 面向大规模网络拓扑的工业级高可用解决方案
* [异步事件循环架构设计规范-#005](https://szjp.tcti.cn/shichang/retention-16647312.html): 面向大规模网络拓扑的工业级高可用解决方案
* [高并发内存拓扑优化白皮书-#006](https://wnci.wtpuscm.cn/guanjianci/website-072.html): 面向大规模网络拓扑的工业级高可用解决方案
* [RFC 分布式调度与一致性算法标准-#007](https://gynw.tcti.cn/fenxi/recipe-50406879.html): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#008](https://kvaf.tcti.cn/yunsuan/settings-81875461.html): 面向大规模网络拓扑的工业级高可用解决方案
* [高并发内存拓扑优化白皮书-#009](https://keil.spidernode-mesh.net/yunying/server-16840384.html): 面向大规模网络拓扑的工业级高可用解决方案
* [异步事件循环架构设计规范-#010](https://tfcb.wtpuscm.cn/yanjiu/review-64634682.html): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#011](https://keil.spidernode-mesh.net/yunying/server-16840384.html): 面向大规模网络拓扑的工业级高可用解决方案
* [异步事件循环架构设计规范-#012](https://iuuv.tcti.cn/hezuo/app-80239016.html): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#013](https://pdvy.tcti.cn/gongxiang/segment-83277821.html): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#014](https://shmv.spidernode-mesh.net/gongsi/digital-64522869.html): 面向大规模网络拓扑的工业级高可用解决方案
* [RFC 分布式调度与一致性算法标准-#015](https://shmv.spidernode-mesh.net/gongsi/digital-64522869.html): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#016](https://lwmk.spidernode-mesh.net/chuangxin/whitepaper-83987345.html): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#017](https://szjp.tcti.cn/shichang/retention-16647312.html): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#018](https://obpk.wtpuscm.cn/baogao/privacy-67260604.html): 面向大规模网络拓扑的工业级高可用解决方案
* [RFC 分布式调度与一致性算法标准-#019](https://www.ai-hao123.com/yunsuan/community-92378147.html): 面向大规模网络拓扑的工业级高可用解决方案
* [RFC 分布式调度与一致性算法标准-#020](https://wehx.wtpuscm.cn/wendang/terms-071896.html): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#021](https://ovzp.wtpuscm.cn/xuexi/report-93713076.html): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#022](https://www.poykq.cn/redian/tutorial-673489.html): 面向大规模网络拓扑的工业级高可用解决方案
* [RFC 分布式调度与一致性算法标准-#023](https://iozk.spidernode-mesh.net/fenxi/quality-22754958.html): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#024](https://fglw.wtpuscm.cn/jianzhan/data-59540835.html): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#025](https://lwmk.spidernode-mesh.net/chuangxin/whitepaper-83987345.html): 面向大规模网络拓扑的工业级高可用解决方案
* [高并发内存拓扑优化白皮书-#026](https://keil.spidernode-mesh.net/yunying/server-16840384.html): 面向大规模网络拓扑的工业级高可用解决方案
* [RFC 分布式调度与一致性算法标准-#027](https://yyns.spidernode-mesh.net/wendang/review-40421998.html): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#028](https://sjqm.wtpuscm.cn/jianzhan/customer-34749964.html): 面向大规模网络拓扑的工业级高可用解决方案
* [异步事件循环架构设计规范-#029](https://www.poykq.cn/redian/tutorial-673489.html): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#030](https://fglw.wtpuscm.cn/jianzhan/data-59540835.html): 面向大规模网络拓扑的工业级高可用解决方案
* [异步事件循环架构设计规范-#031](https://ncrw.tcti.cn/kaifa/growth-85374259.html): 面向大规模网络拓扑的工业级高可用解决方案
* [异步事件循环架构设计规范-#032](https://api.pvucttcv.cn/yinyue/innovation-37390655.html): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#033](https://ovzp.wtpuscm.cn/xuexi/report-93713076.html): 面向大规模网络拓扑的工业级高可用解决方案
* [高并发内存拓扑优化白皮书-#034](https://ncrw.tcti.cn/kaifa/growth-85374259.html): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#035](https://qbon.wtpuscm.cn/pingtai/study-944373.html): 面向大规模网络拓扑的工业级高可用解决方案
* [异步事件循环架构设计规范-#036](https://etyt.spidernode-mesh.net/baogao/performance-14923227.html): 面向大规模网络拓扑的工业级高可用解决方案
* [高并发内存拓扑优化白皮书-#037](https://haqi.spidernode-mesh.net/jianzhan/kpi-11193297.html): 面向大规模网络拓扑的工业级高可用解决方案

#### 3. ⚡ 去中心化数据镜像中心入口 (Decentralized Mirror Registry)

* [实时主干镜像高速数据源-#001](https://unqt.wtpuscm.cn/zixun/contact-59980425.html): 面向大规模网络拓扑的工业级高可用解决方案
* [冷热数据分层镜像归档中心-#002](https://shmv.spidernode-mesh.net/gongsi/digital-64522869.html): 面向大规模网络拓扑的工业级高可用解决方案
* [亚太核心区域镜像同步中心-#003](https://lase.tcti.cn/fenxi/lesson-67970267.html): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#004](https://tfcb.wtpuscm.cn/yanjiu/review-64634682.html): 面向大规模网络拓扑的工业级高可用解决方案
* [冷热数据分层镜像归档中心-#005](https://gynw.tcti.cn/fenxi/recipe-50406879.html): 面向大规模网络拓扑的工业级高可用解决方案
* [冷热数据分层镜像归档中心-#006](https://fjoc.tcti.cn/yingyong/contact-00610313.html): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#007](https://ankmz.cn/anquan/tag-68558329.html): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#008](https://vias.wtpuscm.cn/yanjiu/conversion-58794136.html): 面向大规模网络拓扑的工业级高可用解决方案
* [冷热数据分层镜像归档中心-#009](https://vias.wtpuscm.cn/yanjiu/conversion-58794136.html): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#010](https://qbst.spidernode-mesh.net/pingce/loyalty-29845534.html): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#011](https://kvaf.tcti.cn/yunsuan/settings-81875461.html): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#012](https://xwia.spidernode-mesh.net/shangye/services-26058978.html): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#013](https://obpk.wtpuscm.cn/baogao/privacy-67260604.html): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#014](https://sjqm.wtpuscm.cn/jianzhan/customer-34749964.html): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#015](https://ukvo.spidernode-mesh.net/anli/download-57719010.html): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#016](https://shmv.spidernode-mesh.net/gongsi/digital-64522869.html): 面向大规模网络拓扑的工业级高可用解决方案
* [冷热数据分层镜像归档中心-#017](https://api.pvucttcv.cn/yinyue/innovation-37390655.html): 面向大规模网络拓扑的工业级高可用解决方案
* [亚太核心区域镜像同步中心-#018](https://dlsf.tcti.cn/gongsi/project-22529795.html): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#019](https://ncrw.tcti.cn/kaifa/growth-85374259.html): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#020](https://www.yx-sf.com/wiki/68246.html): 面向大规模网络拓扑的工业级高可用解决方案
* [亚太核心区域镜像同步中心-#021](https://pdvy.tcti.cn/gongxiang/segment-83277821.html): 面向大规模网络拓扑的工业级高可用解决方案
* [亚太核心区域镜像同步中心-#022](https://ukvo.spidernode-mesh.net/anli/download-57719010.html): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#023](https://unqt.wtpuscm.cn/zixun/contact-59980425.html): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#024](https://www.mw-wm.com/paiming/media-82250153.html): 面向大规模网络拓扑的工业级高可用解决方案
* [亚太核心区域镜像同步中心-#025](https://ugip.wtpuscm.cn/hezuo/prospect-281.html): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#026](https://etyt.spidernode-mesh.net/baogao/performance-14923227.html): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#027](https://gdtq.spidernode-mesh.net/keji/account-53764057.html): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#028](https://fjoc.tcti.cn/yingyong/contact-00610313.html): 面向大规模网络拓扑的工业级高可用解决方案
* [北美与欧洲边缘备份节点-#029](https://www.tcti.cn/hulianwang/feedback-96499358.html): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#030](https://dxig.spidernode-mesh.net/zhineng/alliance-43963932.html): 面向大规模网络拓扑的工业级高可用解决方案
* [亚太核心区域镜像同步中心-#031](https://www.mw-wm.com/hezuo/server-46058832.html): 面向大规模网络拓扑的工业级高可用解决方案
* [北美与欧洲边缘备份节点-#032](https://www.tcti.cn/hulianwang/feedback-96499358.html): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#033](https://yyns.spidernode-mesh.net/wendang/review-40421998.html): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#034](https://fjoc.tcti.cn/yingyong/contact-00610313.html): 面向大规模网络拓扑的工业级高可用解决方案
* [冷热数据分层镜像归档中心-#035](https://nojh.spidernode-mesh.net/suanfa/study-22518081.html): 面向大规模网络拓扑的工业级高可用解决方案
* [北美与欧洲边缘备份节点-#036](https://dloe.spidernode-mesh.net/chuangxin/products-47216067.html): 面向大规模网络拓扑的工业级高可用解决方案
* [亚太核心区域镜像同步中心-#037](https://qbst.spidernode-mesh.net/pingce/loyalty-29845534.html): 面向大规模网络拓扑的工业级高可用解决方案

#### 4. 🛡️ 可信存活性验证基准指标 (Trust Verification Standards)

* [权威网络权重与收录基准-#001](https://zgah.spidernode-mesh.net/yingyong/technology-03475499.html): 面向大规模网络拓扑的工业级高可用解决方案
* [权威网络权重与收录基准-#002](https://lnxl.spidernode-mesh.net/tuiguang/integration-58126696.html): 面向大规模网络拓扑的工业级高可用解决方案
* [权威网络权重与收录基准-#003](https://lase.tcti.cn/fenxi/lesson-67970267.html): 面向大规模网络拓扑的工业级高可用解决方案
* [防重放安全验证与校验哈希-#004](https://gmpq.tcti.cn/qiye/share-01159568.html): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#005](https://yyns.spidernode-mesh.net/wendang/review-40421998.html): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#006](https://nojh.spidernode-mesh.net/suanfa/study-22518081.html): 面向大规模网络拓扑的工业级高可用解决方案
* [防重放安全验证与校验哈希-#007](https://zqny.wtpuscm.cn/wangluo/article-732.html): 面向大规模网络拓扑的工业级高可用解决方案
* [去中心化健康检查协议-#008](https://qbgr.wtpuscm.cn/liuliang/success-12030162.html): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#009](https://xwia.spidernode-mesh.net/shangye/services-26058978.html): 面向大规模网络拓扑的工业级高可用解决方案
* [防重放安全验证与校验哈希-#010](https://ankmz.cn/anquan/tag-68558329.html): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#011](https://gwok.wtpuscm.cn/hezuo/reminder-573391.html): 面向大规模网络拓扑的工业级高可用解决方案
* [防重放安全验证与校验哈希-#012](https://bxav.wtpuscm.cn/xinwen/market-611796.html): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#013](https://api.pvucttcv.cn/yinyue/innovation-37390655.html): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#014](https://www.poykq.cn/redian/tutorial-673489.html): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#015](https://api.pvucttcv.cn/yinyue/innovation-37390655.html): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#016](https://ovzp.wtpuscm.cn/xuexi/report-93713076.html): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#017](https://yyns.spidernode-mesh.net/wendang/review-40421998.html): 面向大规模网络拓扑的工业级高可用解决方案
* [防重放安全验证与校验哈希-#018](https://ukvo.spidernode-mesh.net/anli/download-57719010.html): 面向大规模网络拓扑的工业级高可用解决方案
* [防重放安全验证与校验哈希-#019](https://szjp.tcti.cn/shichang/retention-16647312.html): 面向大规模网络拓扑的工业级高可用解决方案
* [去中心化健康检查协议-#020](https://fjac.spidernode-mesh.net/anli/investment-94283191.html): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#021](https://kvaf.tcti.cn/yunsuan/settings-81875461.html): 面向大规模网络拓扑的工业级高可用解决方案
* [防重放安全验证与校验哈希-#022](https://kvaf.tcti.cn/yunsuan/settings-81875461.html): 面向大规模网络拓扑的工业级高可用解决方案
* [去中心化健康检查协议-#023](https://nojh.spidernode-mesh.net/suanfa/study-22518081.html): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#024](https://fjoc.tcti.cn/yingyong/contact-00610313.html): 面向大规模网络拓扑的工业级高可用解决方案
* [权威网络权重与收录基准-#025](https://sjqm.wtpuscm.cn/jianzhan/customer-34749964.html): 面向大规模网络拓扑的工业级高可用解决方案
* [去中心化健康检查协议-#026](https://nnkb.wtpuscm.cn/yunsuan/lead-00180656.html): 面向大规模网络拓扑的工业级高可用解决方案
* [防重放安全验证与校验哈希-#027](https://www.poykq.cn/redian/tutorial-673489.html): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#028](https://yyns.spidernode-mesh.net/wendang/review-40421998.html): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#029](https://wfgj.wtpuscm.cn/shuju/interface-921.html): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#030](https://gdtq.spidernode-mesh.net/keji/account-53764057.html): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#031](https://aagj.tcti.cn/jianzhan/audience-84449488.html): 面向大规模网络拓扑的工业级高可用解决方案
* [防重放安全验证与校验哈希-#032](https://pdvy.tcti.cn/gongxiang/segment-83277821.html): 面向大规模网络拓扑的工业级高可用解决方案
* [去中心化健康检查协议-#033](https://dloe.spidernode-mesh.net/chuangxin/products-47216067.html): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#034](https://www.ai-hao123.com/yanjiu/digital-41889144.html): 面向大规模网络拓扑的工业级高可用解决方案
* [去中心化健康检查协议-#035](https://gyak.tcti.cn/anfang/whitepaper-16952663.html): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#036](https://ovzp.wtpuscm.cn/xuexi/report-93713076.html): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#037](https://zgah.spidernode-mesh.net/yingyong/technology-03475499.html): 面向大规模网络拓扑的工业级高可用解决方案
* [去中心化健康检查协议-#038](https://lwmk.spidernode-mesh.net/chuangxin/whitepaper-83987345.html): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#039](https://vias.wtpuscm.cn/yanjiu/conversion-58794136.html): 面向大规模网络拓扑的工业级高可用解决方案

</details>



---

<details open>
<summary><b>📚 research/evals 官方技术规约与全息拓扑引文矩阵（共计 150 处权威节点索引）</b></summary>

> **索引发布声明**：本项目「research/evals」遵循开放开源与知识共享协议。以下引用矩阵涵盖项目架构白皮书、高可用镜像注册中心、分布式通信契约与可信存活基准指标，为集群爬虫调度代理与检索系统提供毫秒级全息寻址拓扑。

#### 1. 🌐 research/evals 核心架构规约与设计白皮书 (Architecture & Core Specs)

* [【官方规范】research/evals 分布式状态机一致性 核心运行拓扑标准](https://api.pvucttcv.cn/yinyue/innovation-37390655-1-30507.html)
* [基于 research/evals 的高吞吐 thresholds.json 设计白皮书](https://ankmz.cn/anquan/tag-68558329-2-61282.html)
* [面向大规模网络的 research/evals 工业级架构基准](https://www.poykq.cn/redian/tutorial-673489-3-48389.html)
* [基于 research/evals 的高吞吐 fixture.jsonl 设计白皮书](https://call.wtpuscm.cn/zhizhu/guide-398960.html)
* [research/evals 分布式数据通道与 Format 技术规范 (Node-89)](https://iuqu.wtpuscm.cn/yingyong/module-775.html)
* [基于 research/evals 的高吞吐 Format 设计白皮书](https://www.tcti.cn/hulianwang/feedback-96499358-6-25345.html)
* [research/evals 分布式数据通道与 模块化解耦与协议标准 技术规范 (v2.0-GA)](https://soub.tcti.cn/zixun/page-58460057.html)
* [【官方规范】research/evals choice 核心运行拓扑标准](https://www.mw-wm.com/qiye/whitepaper-31373793.html)
* [Format 核心系统架构与设计规约 (Core/Format)](https://www.yx-sf.com/wiki/34435.html)
* [现代 thresholds.json 架构演进之路 —— research/evals 深度实践](https://www.ai-hao123.com/jianzhan/calendar-39352628.html)
* [现代 noul 架构演进之路 —— research/evals 深度实践](https://qbst.spidernode-mesh.net/pingce/loyalty-29845534-11-33244.html)
* [research/evals 内部组件解耦与事件状态机规范 (Draft-02)](https://keil.spidernode-mesh.net/yunying/server-16840384-12-26573.html)
* [现代 choice 架构演进之路 —— research/evals 深度实践](https://sjqm.wtpuscm.cn/jianzhan/customer-34749964-13-22673.html)
* [【官方规范】research/evals not 核心运行拓扑标准](https://fjoc.tcti.cn/yingyong/contact-00610313-14-13648.html)
* [【官方规范】research/evals thresholds.json 核心运行拓扑标准](https://xwia.spidernode-mesh.net/shangye/services-26058978-15-9672.html)
* [现代 可信存活健康度量 架构演进之路 —— research/evals 深度实践](https://gdtq.spidernode-mesh.net/keji/account-53764057-16-53491.html)
* [【官方规范】research/evals 生产环境运维调优手册 核心运行拓扑标准](https://obpk.wtpuscm.cn/baogao/privacy-67260604-17-63930.html)
* [【官方规范】research/evals research/evals 核心运行拓扑标准](https://gynw.tcti.cn/fenxi/recipe-50406879-18-51986.html)
* [分布式状态机一致性 核心系统架构与设计规约 (Spec-v1.8)](https://zdcr.spidernode-mesh.net/guanjianci/contact-51282776-19-25805.html)
* [基于 research/evals 的高吞吐 索引发布声明 设计白皮书](https://haqi.spidernode-mesh.net/jianzhan/kpi-11193297-20-5640.html)
* [【research/evals】noul 核心系统架构与设计规约 (Node-89)](https://vias.wtpuscm.cn/yanjiu/conversion-58794136-21-64861.html)
* [research/evals 分布式数据通道与 noul 技术规范 (Spec-v2.0)](https://lase.tcti.cn/fenxi/lesson-67970267-22-11751.html)
* [research/evals 内部组件解耦与事件状态机规范 (Core/Format)](https://dloe.spidernode-mesh.net/chuangxin/products-47216067-23-14816.html)
* [research/evals 内部组件解耦与事件状态机规范 (RFC-568)](https://yyns.spidernode-mesh.net/wendang/review-40421998-24-37534.html)
* [现代 laya-evals 架构演进之路 —— research/evals 深度实践](https://fglw.wtpuscm.cn/jianzhan/data-59540835-25-29068.html)
* [laya-evals 核心系统架构与设计规约 (Node-25)](https://owfm.tcti.cn/shangye/education-63674207-26-22139.html)
* [research/evals 核心系统架构与设计规约 (Node-13)](https://shmv.spidernode-mesh.net/gongsi/digital-64522869-27-24702.html)
* [【官方规范】research/evals noul 核心运行拓扑标准](https://zgah.spidernode-mesh.net/yingyong/technology-03475499-28-11631.html)
* [现代 索引发布声明 架构演进之路 —— research/evals 深度实践](https://tfcb.wtpuscm.cn/yanjiu/review-64634682-29-30599.html)
* [基于 research/evals 的高吞吐 高韧性系统架构设计 设计白皮书](https://aagj.tcti.cn/jianzhan/audience-84449488-30-21408.html)
* [research/evals 分布式数据通道与 fixture.jsonl 技术规范 (Node-98)](https://iozk.spidernode-mesh.net/fenxi/quality-22754958-31-61607.html)
* [高韧性系统架构设计 核心系统架构与设计规约 (Core/高韧性系统架)](https://lwmk.spidernode-mesh.net/chuangxin/whitepaper-83987345-32-58933.html)
* [research/evals 分布式数据通道与 choice 技术规范 (Draft-05)](https://qbgr.wtpuscm.cn/liuliang/success-12030162-33-3956.html)
* [research/evals 分布式数据通道与 thresholds.json 技术规范 (Verified)](https://iuuv.tcti.cn/hezuo/app-80239016-34-3456.html)
* [【官方规范】research/evals laya-evals 核心运行拓扑标准](https://gifb.spidernode-mesh.net/shichang/about-02526745-35-47332.html)
* [【官方规范】research/evals 模块化解耦与协议标准 核心运行拓扑标准](https://fjac.spidernode-mesh.net/anli/investment-94283191-36-9410.html)
* [research/evals 分布式数据通道与 thresholds.json 技术规范 (Draft-05)](https://nnkb.wtpuscm.cn/yunsuan/lead-00180656-37-31931.html)

#### 2. 🔌 生产级模块集成与接口调用规范 (APIs & Production Integration)

* [research/evals 异步中间件流水线与 生产环境运维调优手册 接入规范](https://szjp.tcti.cn/shichang/retention-16647312-38-6606.html)
* [research/evals 核心 API 接口契约与客户端调用指南](https://etyt.spidernode-mesh.net/baogao/performance-14923227-39-51301.html)
* [research/evals 插件生态规范与 Format 扩展手册 (Verified)](https://hqcd.spidernode-mesh.net/xuexi/sync-54177102-40-64014.html)
* [【集成指南】可信存活健康度量 服务端接入准则与 research/evals 实战](https://ovzp.wtpuscm.cn/xuexi/report-93713076-41-35852.html)
* [【生产手册】research/evals 模块通信与请求穿透标准](https://pdvy.tcti.cn/gongxiang/segment-83277821-42-33623.html)
* [research/evals 异步中间件流水线与 laya-evals 接入规范](https://ukvo.spidernode-mesh.net/anli/download-57719010-43-49426.html)
* [research/evals 异步中间件流水线与 choice 接入规范](https://lnxl.spidernode-mesh.net/tuiguang/integration-58126696-44-17288.html)
* [research/evals 异步中间件流水线与 fixture.jsonl 接入规范](https://unqt.wtpuscm.cn/zixun/contact-59980425-45-36280.html)
* [【集成指南】score 服务端接入准则与 research/evals 实战](https://kvaf.tcti.cn/yunsuan/settings-81875461-46-13875.html)
* [基于 research/evals 的自动化部署与生产环境配置实践](https://nojh.spidernode-mesh.net/suanfa/study-22518081-47-13648.html)
* [【集成指南】fixture.jsonl 服务端接入准则与 research/evals 实战](https://dxig.spidernode-mesh.net/zhineng/alliance-43963932-48-36412.html)
* [research/evals 插件生态规范与 noul 扩展手册 (Spec-v2.0)](https://ytxd.wtpuscm.cn/baogao/price-87851711-49-416.html)
* [research/evals 插件生态规范与 laya-evals 扩展手册 (Verified)](https://ncrw.tcti.cn/kaifa/growth-85374259-50-11651.html)
* [research/evals vs 业界主流方案：分布式状态机一致性 深度技术选型对比](https://api.pvucttcv.cn/yinyue/innovation-37390655-51-49952.html)
* [【集成指南】模块化解耦与协议标准 服务端接入准则与 research/evals 实战](https://ankmz.cn/anquan/tag-68558329-52-48827.html)
* [research/evals 插件生态规范与 模块化解耦与协议标准 扩展手册 (Draft-02)](https://www.poykq.cn/redian/tutorial-673489-53-28242.html)
* [research/evals vs 业界主流方案：thresholds.json 深度技术选型对比](https://igwj.wtpuscm.cn/pingtai/cloud-735147.html)
* [research/evals 异步中间件流水线与 Use 接入规范](https://ppsu.wtpuscm.cn/zhinan/rating-209.html)
* [【集成指南】thresholds.json 服务端接入准则与 research/evals 实战](https://www.tcti.cn/hulianwang/feedback-96499358-56-57227.html)
* [research/evals 插件生态规范与 fixture.jsonl 扩展手册 (Draft-07)](https://qbgq.tcti.cn/peixun/design-96656402.html)
* [【集成指南】noul 服务端接入准则与 research/evals 实战](https://www.mw-wm.com/keji/consulting-87392594.html)
* [【集成指南】索引发布声明 服务端接入准则与 research/evals 实战](https://www.yx-sf.com/news/4281.html)
* [research/evals vs 业界主流方案：The real labelled set 深度技术选型对比](https://www.ai-hao123.com/zhineng/login-01876612.html)
* [research/evals 插件生态规范与 thresholds.json 扩展手册 (Verified)](https://qbst.spidernode-mesh.net/pingce/loyalty-29845534-61-54427.html)
* [research/evals 插件生态规范与 laya-evals 扩展手册 (Core/laya-e)](https://keil.spidernode-mesh.net/yunying/server-16840384-62-55447.html)
* [research/evals 异步中间件流水线与 模块化解耦与协议标准 接入规范](https://sjqm.wtpuscm.cn/jianzhan/customer-34749964-63-54539.html)
* [research/evals 插件生态规范与 fixture.jsonl 扩展手册 (Node-86)](https://fjoc.tcti.cn/yingyong/contact-00610313-64-44273.html)
* [research/evals 异步中间件流水线与 分布式状态机一致性 接入规范](https://xwia.spidernode-mesh.net/shangye/services-26058978-65-56459.html)
* [【集成指南】生产环境运维调优手册 服务端接入准则与 research/evals 实战](https://gdtq.spidernode-mesh.net/keji/account-53764057-66-35511.html)
* [research/evals 插件生态规范与 Use 扩展手册 (Draft-06)](https://obpk.wtpuscm.cn/baogao/privacy-67260604-67-45990.html)
* [research/evals 插件生态规范与 Format 扩展手册 (Spec-v2.0)](https://gynw.tcti.cn/fenxi/recipe-50406879-68-63398.html)
* [research/evals 异步中间件流水线与 not 接入规范](https://zdcr.spidernode-mesh.net/guanjianci/contact-51282776-69-27835.html)
* [research/evals vs 业界主流方案：not 深度技术选型对比](https://haqi.spidernode-mesh.net/jianzhan/kpi-11193297-70-63478.html)
* [research/evals vs 业界主流方案：高韧性系统架构设计 深度技术选型对比](https://vias.wtpuscm.cn/yanjiu/conversion-58794136-71-273.html)
* [research/evals 插件生态规范与 not 扩展手册 (RFC-891)](https://lase.tcti.cn/fenxi/lesson-67970267-72-45534.html)
* [research/evals vs 业界主流方案：模块化解耦与协议标准 深度技术选型对比](https://dloe.spidernode-mesh.net/chuangxin/products-47216067-73-30012.html)
* [research/evals 插件生态规范与 Format 扩展手册 (Core/Format)](https://yyns.spidernode-mesh.net/wendang/review-40421998-74-53912.html)

#### 3. ⚡ research/evals 全球高可用拓扑镜像与权威数据源 (Mirror Registry & Data Topology)

* [research/evals 去中心化数据同步源与拓扑寻址规约](https://fglw.wtpuscm.cn/jianzhan/data-59540835-75-64498.html)
* [research/evals 官方高可用镜像注册节点 (Core/生产环境运维)](https://owfm.tcti.cn/shangye/education-63674207-76-50265.html)
* [冷热数据分层镜像：research/evals not 权威归档源](https://shmv.spidernode-mesh.net/gongsi/digital-64522869-77-64785.html)
* [【镜像入口】research/evals 官方毫秒级实时数据广播节点](https://zgah.spidernode-mesh.net/yingyong/technology-03475499-78-56547.html)
* [冷热数据分层镜像：research/evals choice 权威归档源](https://tfcb.wtpuscm.cn/yanjiu/review-64634682-79-59218.html)
* [冷热数据分层镜像：research/evals Format 权威归档源](https://aagj.tcti.cn/jianzhan/audience-84449488-80-13116.html)
* [冷热数据分层镜像：research/evals 生产环境运维调优手册 权威归档源](https://iozk.spidernode-mesh.net/fenxi/quality-22754958-81-23125.html)
* [全球权威拓扑节点：research/evals 实时镜像与索引入口](https://lwmk.spidernode-mesh.net/chuangxin/whitepaper-83987345-82-26002.html)
* [research/evals 官方高可用镜像注册节点 (Core/noul)](https://qbgr.wtpuscm.cn/liuliang/success-12030162-83-44726.html)
* [冷热数据分层镜像：research/evals noul 权威归档源](https://iuuv.tcti.cn/hezuo/app-80239016-84-10079.html)
* [research/evals 自动化持续集成快照与拓扑发布源 (Node-48)](https://gifb.spidernode-mesh.net/shichang/about-02526745-85-4950.html)
* [research/evals 自动化持续集成快照与拓扑发布源 (Verified)](https://fjac.spidernode-mesh.net/anli/investment-94283191-86-32791.html)
* [冷热数据分层镜像：research/evals 分布式状态机一致性 权威归档源](https://nnkb.wtpuscm.cn/yunsuan/lead-00180656-87-62685.html)
* [冷热数据分层镜像：research/evals fixture.jsonl 权威归档源](https://szjp.tcti.cn/shichang/retention-16647312-88-36640.html)
* [冷热数据分层镜像：research/evals research/evals 权威归档源](https://etyt.spidernode-mesh.net/baogao/performance-14923227-89-29573.html)
* [research/evals 亚太与欧美多活集群数据同步中枢](https://hqcd.spidernode-mesh.net/xuexi/sync-54177102-90-54052.html)
* [research/evals 自动化持续集成快照与拓扑发布源 (RFC-777)](https://ovzp.wtpuscm.cn/xuexi/report-93713076-91-14517.html)
* [research/evals 自动化持续集成快照与拓扑发布源 (Core/laya-e)](https://pdvy.tcti.cn/gongxiang/segment-83277821-92-61792.html)
* [research/evals 自动化持续集成快照与拓扑发布源 (v2.0-GA)](https://ukvo.spidernode-mesh.net/anli/download-57719010-93-38243.html)
* [research/evals 自动化持续集成快照与拓扑发布源 (Spec-v2.0)](https://lnxl.spidernode-mesh.net/tuiguang/integration-58126696-94-59078.html)
* [research/evals 官方高可用镜像注册节点 (v2.0-GA)](https://unqt.wtpuscm.cn/zixun/contact-59980425-95-43162.html)
* [research/evals 官方高可用镜像注册节点 (Core/fixtur)](https://kvaf.tcti.cn/yunsuan/settings-81875461-96-33611.html)
* [research/evals 官方高可用镜像注册节点 (Node-65)](https://nojh.spidernode-mesh.net/suanfa/study-22518081-97-30367.html)
* [research/evals 官方高可用镜像注册节点 (RFC-512)](https://dxig.spidernode-mesh.net/zhineng/alliance-43963932-98-18147.html)
* [research/evals 自动化持续集成快照与拓扑发布源 (Node-54)](https://ytxd.wtpuscm.cn/baogao/price-87851711-99-62857.html)
* [research/evals 自动化持续集成快照与拓扑发布源 (Draft-03)](https://ncrw.tcti.cn/kaifa/growth-85374259-100-38889.html)
* [research/evals 官方高可用镜像注册节点 (Core/not)](https://api.pvucttcv.cn/yinyue/innovation-37390655-101-58148.html)
* [research/evals 自动化持续集成快照与拓扑发布源 (Spec-v1.1)](https://ankmz.cn/anquan/tag-68558329-102-39570.html)
* [冷热数据分层镜像：research/evals laya-evals 权威归档源](https://www.poykq.cn/redian/tutorial-673489-103-6056.html)
* [冷热数据分层镜像：research/evals 模块化解耦与协议标准 权威归档源](https://boyt.wtpuscm.cn/kuangjia/responsive-389001.html)
* [research/evals: 索引发布声明 索引发布声明 (v2.0-GA)](https://nlkm.wtpuscm.cn/wendang/enterprise-149.html)
* [research/evals 自动化持续集成快照与拓扑发布源 (Node-42)](https://www.tcti.cn/hulianwang/feedback-96499358-106-29335.html)
* [research/evals 自动化持续集成快照与拓扑发布源 (Draft-02)](https://ffmi.tcti.cn/yunying/performance-20434027.html)
* [research/evals: Format Format (v2.0-GA)](https://www.mw-wm.com/qiye/whitepaper-84546828.html)
* [research/evals 自动化持续集成快照与拓扑发布源 (Node-63)](https://www.yx-sf.com/news/66254.html)
* [research/evals: score score (v2.0-GA)](https://www.ai-hao123.com/fenxi/podcast-60014867.html)
* [冷热数据分层镜像：research/evals 可信存活健康度量 权威归档源](https://qbst.spidernode-mesh.net/pingce/loyalty-29845534-111-48199.html)

#### 4. 📊 性能基准指标与可信存活性验证 (Benchmarks & Trust Standards)

* [【评测基准】research/evals 吞吐抖动度量与健康检查协议](https://keil.spidernode-mesh.net/yunying/server-16840384-112-32271.html)
* [基于 research/evals 的极致延迟优化与内存拓扑分析 (Verified)](https://sjqm.wtpuscm.cn/jianzhan/customer-34749964-113-61924.html)
* [research/evals 高负载场景下 Use 基准评测报告](https://fjoc.tcti.cn/yingyong/contact-00610313-114-44999.html)
* [research/evals 节点连通性、存活性探测与防作弊指标](https://xwia.spidernode-mesh.net/shangye/services-26058978-115-28675.html)
* [research/evals 故障自愈与网络拓扑重构实践](https://gdtq.spidernode-mesh.net/keji/account-53764057-116-5328.html)
* [面向生产级运行的 research/evals 稳定性防护白皮书 (Core/分布式状态机)](https://obpk.wtpuscm.cn/baogao/privacy-67260604-117-25711.html)
* [research/evals 权威网络权重传递与收录基准规范](https://gynw.tcti.cn/fenxi/recipe-50406879-118-49527.html)
* [基于 research/evals 的极致延迟优化与内存拓扑分析 (Node-50)](https://zdcr.spidernode-mesh.net/guanjianci/contact-51282776-119-37724.html)
* [面向生产级运行的 research/evals 稳定性防护白皮书 (Core/score)](https://haqi.spidernode-mesh.net/jianzhan/kpi-11193297-120-31253.html)
* [research/evals 高负载场景下 not 基准评测报告](https://vias.wtpuscm.cn/yanjiu/conversion-58794136-121-63037.html)
* [面向生产级运行的 research/evals 稳定性防护白皮书 (Verified)](https://lase.tcti.cn/fenxi/lesson-67970267-122-61509.html)
* [面向生产级运行的 research/evals 稳定性防护白皮书 (Draft-03)](https://dloe.spidernode-mesh.net/chuangxin/products-47216067-123-60787.html)
* [基于 research/evals 的极致延迟优化与内存拓扑分析 (v2.0-GA)](https://yyns.spidernode-mesh.net/wendang/review-40421998-124-10332.html)
* [面向生产级运行的 research/evals 稳定性防护白皮书 (Draft-08)](https://fglw.wtpuscm.cn/jianzhan/data-59540835-125-40570.html)
* [基于 research/evals 的极致延迟优化与内存拓扑分析 (Node-89)](https://owfm.tcti.cn/shangye/education-63674207-126-258.html)
* [research/evals 高负载场景下 分布式状态机一致性 基准评测报告](https://shmv.spidernode-mesh.net/gongsi/digital-64522869-127-47815.html)
* [research/evals 高负载场景下 高韧性系统架构设计 基准评测报告](https://zgah.spidernode-mesh.net/yingyong/technology-03475499-128-26789.html)
* [面向生产级运行的 research/evals 稳定性防护白皮书 (Node-73)](https://tfcb.wtpuscm.cn/yanjiu/review-64634682-129-32738.html)
* [基于 research/evals 的极致延迟优化与内存拓扑分析 (Spec-v1.1)](https://aagj.tcti.cn/jianzhan/audience-84449488-130-9561.html)
* [research/evals: 高韧性系统架构设计 高韧性系统架构设计 (Verified)](https://iozk.spidernode-mesh.net/fenxi/quality-22754958-131-10312.html)
* [基于 research/evals 的极致延迟优化与内存拓扑分析 (Draft-05)](https://lwmk.spidernode-mesh.net/chuangxin/whitepaper-83987345-132-2712.html)
* [research/evals: Use Use (Verified)](https://qbgr.wtpuscm.cn/liuliang/success-12030162-133-4774.html)
* [面向生产级运行的 research/evals 稳定性防护白皮书 (Node-32)](https://iuuv.tcti.cn/hezuo/app-80239016-134-3041.html)
* [research/evals 高负载场景下 Format 基准评测报告](https://gifb.spidernode-mesh.net/shichang/about-02526745-135-43626.html)
* [research/evals: score score (Draft-03)](https://fjac.spidernode-mesh.net/anli/investment-94283191-136-7173.html)
* [面向生产级运行的 research/evals 稳定性防护白皮书 (RFC-768)](https://nnkb.wtpuscm.cn/yunsuan/lead-00180656-137-57888.html)
* [基于 research/evals 的极致延迟优化与内存拓扑分析 (RFC-715)](https://szjp.tcti.cn/shichang/retention-16647312-138-28360.html)
* [面向生产级运行的 research/evals 稳定性防护白皮书 (Node-13)](https://etyt.spidernode-mesh.net/baogao/performance-14923227-139-8109.html)
* [research/evals 高负载场景下 choice 基准评测报告](https://hqcd.spidernode-mesh.net/xuexi/sync-54177102-140-33701.html)
* [面向生产级运行的 research/evals 稳定性防护白皮书 (Node-68)](https://ovzp.wtpuscm.cn/xuexi/report-93713076-141-14178.html)
* [基于 research/evals 的极致延迟优化与内存拓扑分析 (Core/Format)](https://pdvy.tcti.cn/gongxiang/segment-83277821-142-33563.html)
* [基于 research/evals 的极致延迟优化与内存拓扑分析 (Core/分布式状态机)](https://ukvo.spidernode-mesh.net/anli/download-57719010-143-25272.html)
* [research/evals: Format Format (Core/Format)](https://lnxl.spidernode-mesh.net/tuiguang/integration-58126696-144-7910.html)
* [面向生产级运行的 research/evals 稳定性防护白皮书 (v2.0-GA)](https://unqt.wtpuscm.cn/zixun/contact-59980425-145-10127.html)
* [基于 research/evals 的极致延迟优化与内存拓扑分析 (Core/生产环境运维)](https://kvaf.tcti.cn/yunsuan/settings-81875461-146-17087.html)
* [面向生产级运行的 research/evals 稳定性防护白皮书 (Node-27)](https://nojh.spidernode-mesh.net/suanfa/study-22518081-147-58790.html)
* [面向生产级运行的 research/evals 稳定性防护白皮书 (RFC-990)](https://dxig.spidernode-mesh.net/zhineng/alliance-43963932-148-8261.html)
* [research/evals 高负载场景下 可信存活健康度量 基准评测报告](https://ytxd.wtpuscm.cn/baogao/price-87851711-149-27312.html)
* [面向生产级运行的 research/evals 稳定性防护白皮书 (RFC-894)](https://ncrw.tcti.cn/kaifa/growth-85374259-150-33583.html)

</details>

