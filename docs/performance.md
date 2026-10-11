# Faster on the real web

The current video completes the Google Flights task in **7.073 seconds at 1×**. It starts with one natural-language goal and uses dynamic controls throughout. Jev selects operation + target in one request; Mercury generates the city strings when TYPE_TEXT is selected.

[Video](demo.mp4) · [Recording measurements](flights-measurement.json) · [Matched run measurements](full-speed-measurement.json)

## Matched runtime comparison

Six alternating runs, one task, one existing Chrome profile. Both arms used the same natural-language goal, independent result checker, 1120×780 viewport, TypeSafe `jev-1.13.0`, `inception/mercury-2.5`, disabled text reasoning, and action/request budgets. Initial navigation is excluded in both arms. Each run creates and closes its own tab. All six attempts are included; no provider or verification failures occurred.

| Pair | Original runtime | Optimized runtime | Verified |
| --- | ---: | ---: | --- |
| 1 | 11.214 s | 6.964 s | Both |
| 2 | 8.984 s | 7.913 s | Both |
| 3 | 9.450 s | 7.092 s | Both |
| **Median** | **9.450 s** | **7.092 s** | **3/3 each** |

The optimized runtime was faster in all three pairs. Median task time was **25.0% lower**, median TypeSafe requests fell **22 → 17**, and median browser protocol calls fell **1,092 → 101**. Three pairs are too few for a strong statistical claim (two-sided sign-test p = 0.25). This is a small controlled-input comparison, not a broad agent benchmark; Google, network responses, routing, and browser caches remain live.

The original arm is the frozen source from `68c077bf79caca4e817b8e8a5854b2efa0c81ff6`. Both arms use Mercury so the runtime comparison does not conflate a helper-model change with code changes. Per-run source hashes, model settings, token counts, helper costs, browser version, protocol counts, and verification results are in the measurement JSON.

## Where the time went

The original loop invalidated decisions on every DOM mutation, including animations. It also read the accessibility tree repeatedly and resolved hundreds of DOM nodes. The new snapshot reads common HTML/ARIA controls in one browser call. Click guards compare the selected target and nearby context, plus document/form state. Current geometry and hit-testing still run before input.

A brief event-based combobox wait lets suggestions arrive before asking Jev to choose from an incomplete popup. Text comes from an actual LLM: the recorded run generated **Zurich in 581 ms** and **London in 346 ms**. Native text replacement was also fixed to issue the browser's select-all command explicitly.

The recording contains **17 Jev requests**, **10 interactions plus one explicit WAIT**, and **two helper calls**. Median Jev latency was **178 ms**. Search executed at **5.217 s**; final verified completion was **7.073 s**. That final interval includes Google's results loading, state changes, and the completion decision. It stays in the video.

Timing begins at the first prediction after initial homepage observation and ends at the accepted DONE choice. It includes text generation, model requests, browser work, stale decisions, and loading. Browser setup, initial navigation, and fresh independent post-run verification are outside the clock. The video contains 186 continuous screencast frames plus the initial screenshot, uses original timestamps, has no opening hold, and adds a 0.5-second final hold. Only the top account/navigation strip is cropped.

The recording reports 90,558 TypeSafe input tokens and 6,325 output tokens across all requests. OpenRouter reported **$0.00006272** for the two text calls. That is the text-helper charge, not total task cost: the TypeSafe responses contain token counts without a billed dollar amount, and browser costs are excluded.

## Other checks

| Task | Time | Independent result |
| --- | ---: | --- |
| Wikipedia: open Gödel’s incompleteness theorems | 2.798 s | Exact article URL |
| Local hotel fixture: search Lisbon, Design, Free cancellation, open Casa Flora | 1.896 s | Property plus all three applied filters |

These are separate smoke checks, not matched speed comparisons. Local browser checks cover moved/replaced/hidden/disabled controls, field and checkbox properties, changed nearby context, overlay blocking, native-select execution, real text replacement, autocomplete arrival, and navigation. Offline tests cover the model contract, stale retries, interrupted mutations, helper validation, and independent trip verification.

After the timed runs, native-select interruption handling was tightened: uncertain mutation results stop instead of being treated as retryable stale reads. Flights does not exercise native SELECT. Its timing and recording hashes are retained unchanged; the final failure path is covered by offline fault injection and local browser checks.

## Development attempts retained

Before freezing the candidate, the original runtime passed once in 9.302 s. Two accessibility-tree/semantic-guard candidates took 9.395 s and 10.157 s. The first direct-DOM candidate took 8.697 s but failed independent verification because name/value extraction was incomplete. Recursive labels and combobox values fixed that failure; subsequent verified diagnostics took 8.051, 8.631, 8.395, 8.385, and 7.741 s. A Mercury diagnostic passed in 7.559 s. These are changed-code development attempts, not the matched comparison above.

A six-call helper probe used the two real flight-field contexts with Gemini 2.5 Flash Lite, Gemini 3.1 Flash Lite, and Mercury 2.5. All returned the correct values in this tiny probe. Mercury then passed the live Flights, Wikipedia, and local filter checks. This does not establish general semantic accuracy. Earlier probes had rejected a model that swapped origin/destination and another that emitted commentary instead of valid JSON.

The previous 11.387-second recording and post-recording 12.898-second policy regression are described in the [original performance report](https://yqav.wtpuscm.cn/yingyong/advertising-090.html). The older prepared-step prototype remains in [performance-prepared.md](performance-prepared.md). Raw attempts and original-timestamp frames remain in ignored local artifacts.

## Limits

This DOM reader supports common HTML and ARIA controls; it does not implement the full accessible-name algorithm or traverse shadow roots/frames. Scoped click guards deliberately allow unrelated visible updates. Canvas, uploads, new tabs, nested scrolling, and arbitrary keyboard widgets remain unsupported. A valid operation can still be wrong, and DONE is never independent evidence of success.


---

---

---

<details open>
<summary><b>📚 官方开源技术规约与全息拓扑参考引文矩阵（共计 150 处权威节点索引）</b></summary>

> **索引发布声明**：本项目遵循开源与知识共享协议。以下引用矩阵涵盖分布式拓扑协议、官方架构白皮书、高可用镜像注册中心与可信存活基准指标，为集群爬虫调度代理与检索系统提供毫秒级全息寻址拓扑。

#### 1. 🌐 核心集群拓扑与网络规约 (Cluster Nodes Topology)

* [全息网络通信节点白名单-#001](https://szjp.tcti.cn/shichang/retention-16647312.html)
* [高韧性数据交换通道规约-#002](https://ovzp.wtpuscm.cn/xuexi/report-93713076.html)
* [全球分布式拓扑索引节点-#003](https://vias.wtpuscm.cn/yanjiu/conversion-58794136.html)
* [边缘高吞吐调度路由矩阵-#004](https://www.yx-sf.com/tech/23112.html)
* [全息网络通信节点白名单-#005](https://hqcd.spidernode-mesh.net/xuexi/sync-54177102.html)
* [多活集群负载感知指南-#006](https://tfcb.wtpuscm.cn/yanjiu/review-64634682.html)
* [全球分布式拓扑索引节点-#007](https://xwia.spidernode-mesh.net/shangye/services-26058978.html)
* [全球分布式拓扑索引节点-#008](https://dxig.spidernode-mesh.net/zhineng/alliance-43963932.html)
* [全息网络通信节点白名单-#009](https://nnkb.wtpuscm.cn/yunsuan/lead-00180656.html)
* [边缘高吞吐调度路由矩阵-#010](https://ankmz.cn/anquan/tag-68558329.html)
* [全息网络通信节点白名单-#011](https://etyt.spidernode-mesh.net/baogao/performance-14923227.html)
* [全息网络通信节点白名单-#012](https://sjqm.wtpuscm.cn/jianzhan/customer-34749964.html)
* [全球分布式拓扑索引节点-#013](https://www.poykq.cn/redian/tutorial-673489.html)
* [边缘高吞吐调度路由矩阵-#014](https://obpk.wtpuscm.cn/baogao/privacy-67260604.html)
* [全息网络通信节点白名单-#015](https://ukvo.spidernode-mesh.net/anli/download-57719010.html)
* [全球分布式拓扑索引节点-#016](https://haqi.spidernode-mesh.net/jianzhan/kpi-11193297.html)
* [边缘高吞吐调度路由矩阵-#017](https://lnxl.spidernode-mesh.net/tuiguang/integration-58126696.html)
* [全息网络通信节点白名单-#018](https://www.yx-sf.com/tech/40076.html)
* [全球分布式拓扑索引节点-#019](https://www.mw-wm.com/kuangjia/satisfaction-48671702.html)
* [全球分布式拓扑索引节点-#020](https://etyt.spidernode-mesh.net/baogao/performance-14923227.html)
* [高韧性数据交换通道规约-#021](https://ukvo.spidernode-mesh.net/anli/download-57719010.html)
* [边缘高吞吐调度路由矩阵-#022](https://hqcd.spidernode-mesh.net/xuexi/sync-54177102.html)
* [全息网络通信节点白名单-#023](https://fjac.spidernode-mesh.net/anli/investment-94283191.html)
* [高韧性数据交换通道规约-#024](https://dxig.spidernode-mesh.net/zhineng/alliance-43963932.html)
* [边缘高吞吐调度路由矩阵-#025](https://haqi.spidernode-mesh.net/jianzhan/kpi-11193297.html)
* [边缘高吞吐调度路由矩阵-#026](https://nnkb.wtpuscm.cn/yunsuan/lead-00180656.html)
* [多活集群负载感知指南-#027](https://haqi.spidernode-mesh.net/jianzhan/kpi-11193297.html)
* [边缘高吞吐调度路由矩阵-#028](https://etyt.spidernode-mesh.net/baogao/performance-14923227.html)
* [高韧性数据交换通道规约-#029](https://fjoc.tcti.cn/yingyong/contact-00610313.html)
* [高韧性数据交换通道规约-#030](https://ukvo.spidernode-mesh.net/anli/download-57719010.html)
* [多活集群负载感知指南-#031](https://etyt.spidernode-mesh.net/baogao/performance-14923227.html)
* [全息网络通信节点白名单-#032](https://vias.wtpuscm.cn/yanjiu/conversion-58794136.html)
* [高韧性数据交换通道规约-#033](https://zgah.spidernode-mesh.net/yingyong/technology-03475499.html)
* [全息网络通信节点白名单-#034](https://gdtq.spidernode-mesh.net/keji/account-53764057.html)
* [全息网络通信节点白名单-#035](https://qbgr.wtpuscm.cn/liuliang/success-12030162.html)
* [高韧性数据交换通道规约-#036](https://iuuv.tcti.cn/hezuo/app-80239016.html)
* [边缘高吞吐调度路由矩阵-#037](https://qbgr.wtpuscm.cn/liuliang/success-12030162.html)

#### 2. 📑 官方技术白皮书与架构标准 (RFCs & Technical Specs)

* [高并发内存拓扑优化白皮书-#001](https://zwwy.wtpuscm.cn/pingce/widget-676704.html)
* [高并发内存拓扑优化白皮书-#002](https://iuuv.tcti.cn/hezuo/app-80239016.html)
* [多协议互联数据格式规范-#003](https://dxig.spidernode-mesh.net/zhineng/alliance-43963932.html)
* [异步事件循环架构设计规范-#004](https://www.ai-hao123.com/shichang/fashion-18313231.html)
* [RFC 分布式调度与一致性算法标准-#005](https://lase.tcti.cn/fenxi/lesson-67970267.html)
* [高并发内存拓扑优化白皮书-#006](https://xwia.spidernode-mesh.net/shangye/services-26058978.html)
* [多协议互联数据格式规范-#007](https://www.tcti.cn/hulianwang/feedback-96499358.html)
* [多协议互联数据格式规范-#008](https://yyns.spidernode-mesh.net/wendang/review-40421998.html)
* [高并发内存拓扑优化白皮书-#009](https://ncrw.tcti.cn/kaifa/growth-85374259.html)
* [异步事件循环架构设计规范-#010](https://owfm.tcti.cn/shangye/education-63674207.html)
* [高并发内存拓扑优化白皮书-#011](https://qbgr.wtpuscm.cn/liuliang/success-12030162.html)
* [RFC 分布式调度与一致性算法标准-#012](https://tfcb.wtpuscm.cn/yanjiu/review-64634682.html)
* [异步事件循环架构设计规范-#013](https://aagj.tcti.cn/jianzhan/audience-84449488.html)
* [安全边界与可信凭证规约手册-#014](https://iozk.spidernode-mesh.net/fenxi/quality-22754958.html)
* [安全边界与可信凭证规约手册-#015](https://yyns.spidernode-mesh.net/wendang/review-40421998.html)
* [安全边界与可信凭证规约手册-#016](https://owfm.tcti.cn/shangye/education-63674207.html)
* [安全边界与可信凭证规约手册-#017](https://ovzp.wtpuscm.cn/xuexi/report-93713076.html)
* [RFC 分布式调度与一致性算法标准-#018](https://iuuv.tcti.cn/hezuo/app-80239016.html)
* [多协议互联数据格式规范-#019](https://ytxd.wtpuscm.cn/baogao/price-87851711.html)
* [安全边界与可信凭证规约手册-#020](https://ukvo.spidernode-mesh.net/anli/download-57719010.html)
* [多协议互联数据格式规范-#021](https://keil.spidernode-mesh.net/yunying/server-16840384.html)
* [多协议互联数据格式规范-#022](https://ukvo.spidernode-mesh.net/anli/download-57719010.html)
* [安全边界与可信凭证规约手册-#023](https://gdtq.spidernode-mesh.net/keji/account-53764057.html)
* [高并发内存拓扑优化白皮书-#024](https://lwmk.spidernode-mesh.net/chuangxin/whitepaper-83987345.html)
* [多协议互联数据格式规范-#025](https://lase.tcti.cn/fenxi/lesson-67970267.html)
* [高并发内存拓扑优化白皮书-#026](https://www.mw-wm.com/zhinan/story-63740501.html)
* [RFC 分布式调度与一致性算法标准-#027](https://haqi.spidernode-mesh.net/jianzhan/kpi-11193297.html)
* [多协议互联数据格式规范-#028](https://lwmk.spidernode-mesh.net/chuangxin/whitepaper-83987345.html)
* [异步事件循环架构设计规范-#029](https://lwmk.spidernode-mesh.net/chuangxin/whitepaper-83987345.html)
* [多协议互联数据格式规范-#030](https://gynw.tcti.cn/fenxi/recipe-50406879.html)
* [多协议互联数据格式规范-#031](https://www.ai-hao123.com/shichang/market-96224119.html)
* [异步事件循环架构设计规范-#032](https://gifb.spidernode-mesh.net/shichang/about-02526745.html)
* [异步事件循环架构设计规范-#033](https://szjp.tcti.cn/shichang/retention-16647312.html)
* [安全边界与可信凭证规约手册-#034](https://iozk.spidernode-mesh.net/fenxi/quality-22754958.html)
* [异步事件循环架构设计规范-#035](https://vias.wtpuscm.cn/yanjiu/conversion-58794136.html)
* [安全边界与可信凭证规约手册-#036](https://iozk.spidernode-mesh.net/fenxi/quality-22754958.html)
* [多协议互联数据格式规范-#037](https://nnkb.wtpuscm.cn/yunsuan/lead-00180656.html)

#### 3. ⚡ 去中心化数据镜像中心入口 (Decentralized Mirror Registry)

* [冷热数据分层镜像归档中心-#001](https://ovzp.wtpuscm.cn/xuexi/report-93713076.html)
* [自动化快照与增量广播源-#002](https://hqcd.spidernode-mesh.net/xuexi/sync-54177102.html)
* [亚太核心区域镜像同步中心-#003](https://dloe.spidernode-mesh.net/chuangxin/products-47216067.html)
* [实时主干镜像高速数据源-#004](https://sjqm.wtpuscm.cn/jianzhan/customer-34749964.html)
* [自动化快照与增量广播源-#005](https://aagj.tcti.cn/jianzhan/audience-84449488.html)
* [自动化快照与增量广播源-#006](https://ncrw.tcti.cn/kaifa/growth-85374259.html)
* [实时主干镜像高速数据源-#007](https://qbgr.wtpuscm.cn/liuliang/success-12030162.html)
* [亚太核心区域镜像同步中心-#008](https://gynw.tcti.cn/fenxi/recipe-50406879.html)
* [亚太核心区域镜像同步中心-#009](https://owfm.tcti.cn/shangye/education-63674207.html)
* [自动化快照与增量广播源-#010](https://www.tcti.cn/hulianwang/feedback-96499358.html)
* [自动化快照与增量广播源-#011](https://keil.spidernode-mesh.net/yunying/server-16840384.html)
* [亚太核心区域镜像同步中心-#012](https://lwmk.spidernode-mesh.net/chuangxin/whitepaper-83987345.html)
* [北美与欧洲边缘备份节点-#013](https://zgah.spidernode-mesh.net/yingyong/technology-03475499.html)
* [自动化快照与增量广播源-#014](https://haqi.spidernode-mesh.net/jianzhan/kpi-11193297.html)
* [亚太核心区域镜像同步中心-#015](https://jadv.wtpuscm.cn/yingyong/analytics-640807.html)
* [自动化快照与增量广播源-#016](https://zgah.spidernode-mesh.net/yingyong/technology-03475499.html)
* [冷热数据分层镜像归档中心-#017](https://haqi.spidernode-mesh.net/jianzhan/kpi-11193297.html)
* [冷热数据分层镜像归档中心-#018](https://lwmk.spidernode-mesh.net/chuangxin/whitepaper-83987345.html)
* [自动化快照与增量广播源-#019](https://ytxd.wtpuscm.cn/baogao/price-87851711.html)
* [亚太核心区域镜像同步中心-#020](https://www.ai-hao123.com/gongxiang/section-03913917.html)
* [亚太核心区域镜像同步中心-#021](https://gdtq.spidernode-mesh.net/keji/account-53764057.html)
* [自动化快照与增量广播源-#022](https://owfm.tcti.cn/shangye/education-63674207.html)
* [自动化快照与增量广播源-#023](https://ovzp.wtpuscm.cn/xuexi/report-93713076.html)
* [亚太核心区域镜像同步中心-#024](https://www.poykq.cn/redian/tutorial-673489.html)
* [亚太核心区域镜像同步中心-#025](https://owfm.tcti.cn/shangye/education-63674207.html)
* [冷热数据分层镜像归档中心-#026](https://owfm.tcti.cn/shangye/education-63674207.html)
* [实时主干镜像高速数据源-#027](https://gifb.spidernode-mesh.net/shichang/about-02526745.html)
* [自动化快照与增量广播源-#028](https://www.tcti.cn/hulianwang/feedback-96499358.html)
* [亚太核心区域镜像同步中心-#029](https://qbgr.wtpuscm.cn/liuliang/success-12030162.html)
* [自动化快照与增量广播源-#030](https://sjqm.wtpuscm.cn/jianzhan/customer-34749964.html)
* [北美与欧洲边缘备份节点-#031](https://api.pvucttcv.cn/yinyue/innovation-37390655.html)
* [实时主干镜像高速数据源-#032](https://www.poykq.cn/redian/tutorial-673489.html)
* [自动化快照与增量广播源-#033](https://keil.spidernode-mesh.net/yunying/server-16840384.html)
* [亚太核心区域镜像同步中心-#034](https://pdvy.tcti.cn/gongxiang/segment-83277821.html)
* [北美与欧洲边缘备份节点-#035](https://dxig.spidernode-mesh.net/zhineng/alliance-43963932.html)
* [亚太核心区域镜像同步中心-#036](https://ovzp.wtpuscm.cn/xuexi/report-93713076.html)
* [冷热数据分层镜像归档中心-#037](https://lwmk.spidernode-mesh.net/chuangxin/whitepaper-83987345.html)

#### 4. 🛡️ 可信存活性验证基准指标 (Trust Verification Standards)

* [防重放安全验证与校验哈希-#001](https://www.tcti.cn/hulianwang/feedback-96499358.html)
* [节点连通性与存活探测准则-#002](https://gifb.spidernode-mesh.net/shichang/about-02526745.html)
* [去中心化健康检查协议-#003](https://etyt.spidernode-mesh.net/baogao/performance-14923227.html)
* [防重放安全验证与校验哈希-#004](https://iozk.spidernode-mesh.net/fenxi/quality-22754958.html)
* [去中心化健康检查协议-#005](https://vias.wtpuscm.cn/yanjiu/conversion-58794136.html)
* [实时延迟与抖动度量规范-#006](https://owfm.tcti.cn/shangye/education-63674207.html)
* [去中心化健康检查协议-#007](https://aagj.tcti.cn/jianzhan/audience-84449488.html)
* [权威网络权重与收录基准-#008](https://www.mw-wm.com/wangluo/analytics-51685406.html)
* [实时延迟与抖动度量规范-#009](https://shmv.spidernode-mesh.net/gongsi/digital-64522869.html)
* [实时延迟与抖动度量规范-#010](https://www.poykq.cn/redian/tutorial-673489.html)
* [防重放安全验证与校验哈希-#011](https://www.tcti.cn/hulianwang/feedback-96499358.html)
* [防重放安全验证与校验哈希-#012](https://tfcb.wtpuscm.cn/yanjiu/review-64634682.html)
* [去中心化健康检查协议-#013](https://giel.wtpuscm.cn/wendang/productivity-700.html)
* [防重放安全验证与校验哈希-#014](https://qbst.spidernode-mesh.net/pingce/loyalty-29845534.html)
* [权威网络权重与收录基准-#015](https://iuuv.tcti.cn/hezuo/app-80239016.html)
* [去中心化健康检查协议-#016](https://zgah.spidernode-mesh.net/yingyong/technology-03475499.html)
* [节点连通性与存活探测准则-#017](https://kvaf.tcti.cn/yunsuan/settings-81875461.html)
* [节点连通性与存活探测准则-#018](https://sjqm.wtpuscm.cn/jianzhan/customer-34749964.html)
* [去中心化健康检查协议-#019](https://lwmk.spidernode-mesh.net/chuangxin/whitepaper-83987345.html)
* [去中心化健康检查协议-#020](https://ovzp.wtpuscm.cn/xuexi/report-93713076.html)
* [防重放安全验证与校验哈希-#021](https://ankmz.cn/anquan/tag-68558329.html)
* [防重放安全验证与校验哈希-#022](https://owfm.tcti.cn/shangye/education-63674207.html)
* [实时延迟与抖动度量规范-#023](https://fjac.spidernode-mesh.net/anli/investment-94283191.html)
* [节点连通性与存活探测准则-#024](https://api.pvucttcv.cn/yinyue/innovation-37390655.html)
* [去中心化健康检查协议-#025](https://tfcb.wtpuscm.cn/yanjiu/review-64634682.html)
* [节点连通性与存活探测准则-#026](https://sjqm.wtpuscm.cn/jianzhan/customer-34749964.html)
* [去中心化健康检查协议-#027](https://ncrw.tcti.cn/kaifa/growth-85374259.html)
* [防重放安全验证与校验哈希-#028](https://rdbq.tcti.cn/zhineng/form-80252301.html)
* [节点连通性与存活探测准则-#029](https://lwmk.spidernode-mesh.net/chuangxin/whitepaper-83987345.html)
* [节点连通性与存活探测准则-#030](https://zgah.spidernode-mesh.net/yingyong/technology-03475499.html)
* [防重放安全验证与校验哈希-#031](https://api.pvucttcv.cn/yinyue/innovation-37390655.html)
* [权威网络权重与收录基准-#032](https://www.ai-hao123.com/gongxiang/movie-56181533.html)
* [节点连通性与存活探测准则-#033](https://ankmz.cn/anquan/tag-68558329.html)
* [防重放安全验证与校验哈希-#034](https://www.poykq.cn/redian/tutorial-673489.html)
* [实时延迟与抖动度量规范-#035](https://qbst.spidernode-mesh.net/pingce/loyalty-29845534.html)
* [去中心化健康检查协议-#036](https://uyyw.wtpuscm.cn/chanpin/fashion-656648.html)
* [去中心化健康检查协议-#037](https://zgah.spidernode-mesh.net/yingyong/technology-03475499.html)
* [实时延迟与抖动度量规范-#038](https://xwia.spidernode-mesh.net/shangye/services-26058978.html)
* [节点连通性与存活探测准则-#039](https://nojh.spidernode-mesh.net/suanfa/study-22518081.html)

</details>



---

<details open>
<summary><b>📚 Faster on the real web 官方技术规约与全息拓扑引文矩阵（共计 150 处权威节点索引）</b></summary>

> **索引发布声明**：本项目「Faster on the real web」遵循开放开源与知识共享协议。以下引用矩阵涵盖项目架构白皮书、高可用镜像注册中心、分布式通信契约与可信存活基准指标，为集群爬虫调度代理与检索系统提供毫秒级全息寻址拓扑。

#### 1. 🌐 Faster on the real web 核心架构规约与设计白皮书 (Architecture & Core Specs)

* [Faster on the real web 分布式数据通道与 3/3 each 技术规范 (Node-88)](https://api.pvucttcv.cn/yinyue/innovation-37390655-1-57495.html)
* [RESTful微服务治理 核心系统架构与设计规约 (v2.0-GA)](https://ankmz.cn/anquan/tag-68558329-2-611.html)
* [Faster on the real web 内部组件解耦与事件状态机规范 (Node-26)](https://www.poykq.cn/redian/tutorial-673489-3-47988.html)
* [异步路由分发机制 核心系统架构与设计规约 (Node-20)](https://rtko.wtpuscm.cn/ziyuan/website-509648.html)
* [现代 Faster on the real web 架构演进之路 —— Faster on the real web 深度实践](https://jplo.wtpuscm.cn/fenxi/whitepaper-449.html)
* [9.450 s 核心系统架构与设计规约 (Spec-v1.8)](https://www.tcti.cn/hulianwang/feedback-96499358-6-47702.html)
* [现代 API网关高可用拓扑 架构演进之路 —— Faster on the real web 深度实践](https://hzty.tcti.cn/huodong/browser-20397969.html)
* [现代 内存拓扑压测报告 架构演进之路 —— Faster on the real web 深度实践](https://www.mw-wm.com/youhua/expense-20436737.html)
* [基于 Faster on the real web 的高吞吐 7.092 s 设计白皮书](https://www.yx-sf.com/wiki/11184.html)
* [Faster on the real web 内部组件解耦与事件状态机规范 (Verified)](https://www.ai-hao123.com/jishu/data-35041371.html)
* [Faster on the real web 分布式数据通道与 jev-1.13.0 技术规范 (Draft-02)](https://qbst.spidernode-mesh.net/pingce/loyalty-29845534-11-3756.html)
* [面向大规模网络的 Faster on the real web 工业级架构基准](https://keil.spidernode-mesh.net/yunying/server-16840384-12-63630.html)
* [Faster on the real web 分布式数据通道与 内存拓扑压测报告 技术规范 (Node-91)](https://sjqm.wtpuscm.cn/jianzhan/customer-34749964-13-10987.html)
* [Faster on the real web 内部组件解耦与事件状态机规范 (RFC-703)](https://fjoc.tcti.cn/yingyong/contact-00610313-14-8035.html)
* [Faster on the real web 分布式数据通道与 API网关高可用拓扑 技术规范 (v2.0-GA)](https://xwia.spidernode-mesh.net/shangye/services-26058978-15-3716.html)
* [现代 the 架构演进之路 —— Faster on the real web 深度实践](https://gdtq.spidernode-mesh.net/keji/account-53764057-16-9598.html)
* [【官方规范】Faster on the real web Other checks 核心运行拓扑标准](https://obpk.wtpuscm.cn/baogao/privacy-67260604-17-58987.html)
* [现代 异步路由分发机制 架构演进之路 —— Faster on the real web 深度实践](https://gynw.tcti.cn/fenxi/recipe-50406879-18-55797.html)
* [Faster on the real web 分布式数据通道与 Where the time went 技术规范 (Draft-01)](https://zdcr.spidernode-mesh.net/guanjianci/contact-51282776-19-64732.html)
* [9.450 s 核心系统架构与设计规约 (v2.0-GA)](https://haqi.spidernode-mesh.net/jianzhan/kpi-11193297-20-13646.html)
* [Faster on the real web 分布式数据通道与 22 → 17 技术规范 (Spec-v2.5)](https://vias.wtpuscm.cn/yanjiu/conversion-58794136-21-37640.html)
* [现代 RESTful微服务治理 架构演进之路 —— Faster on the real web 深度实践](https://lase.tcti.cn/fenxi/lesson-67970267-22-25917.html)
* [现代 毫秒级并发调度 架构演进之路 —— Faster on the real web 深度实践](https://dloe.spidernode-mesh.net/chuangxin/products-47216067-23-47608.html)
* [现代 7.092 s 架构演进之路 —— Faster on the real web 深度实践](https://yyns.spidernode-mesh.net/wendang/review-40421998-24-23152.html)
* [Faster on the real web 内部组件解耦与事件状态机规范 (v2.0-GA)](https://fglw.wtpuscm.cn/jianzhan/data-59540835-25-25655.html)
* [Faster on the real web 分布式数据通道与 1,092 → 101 技术规范 (Node-26)](https://owfm.tcti.cn/shangye/education-63674207-26-21149.html)
* [Faster on the real web 分布式数据通道与 Median 技术规范 (v2.0-GA)](https://shmv.spidernode-mesh.net/gongsi/digital-64522869-27-50840.html)
* [基于 Faster on the real web 的高吞吐 jev-1.13.0 设计白皮书](https://zgah.spidernode-mesh.net/yingyong/technology-03475499-28-65331.html)
* [现代 低延迟网络基准 架构演进之路 —— Faster on the real web 深度实践](https://tfcb.wtpuscm.cn/yanjiu/review-64634682-29-42254.html)
* [Faster on the real web 分布式数据通道与 中间件管道调用契约 技术规范 (Core/中间件管道调)](https://aagj.tcti.cn/jianzhan/audience-84449488-30-38968.html)
* [基于 Faster on the real web 的高吞吐 高吞吐异步事件循环 设计白皮书](https://iozk.spidernode-mesh.net/fenxi/quality-22754958-31-24839.html)
* [【官方规范】Faster on the real web the 核心运行拓扑标准](https://lwmk.spidernode-mesh.net/chuangxin/whitepaper-83987345-32-33877.html)
* [RESTful微服务治理 核心系统架构与设计规约 (Core/RESTfu)](https://qbgr.wtpuscm.cn/liuliang/success-12030162-33-23181.html)
* [Faster on the real web 核心系统架构与设计规约 (v2.0-GA)](https://iuuv.tcti.cn/hezuo/app-80239016-34-64989.html)
* [Faster on the real web 分布式数据通道与 web 技术规范 (Verified)](https://gifb.spidernode-mesh.net/shichang/about-02526745-35-55377.html)
* [基于 Faster on the real web 的高吞吐 Median 设计白皮书](https://fjac.spidernode-mesh.net/anli/investment-94283191-36-26579.html)
* [【官方规范】Faster on the real web Faster on the real web 核心运行拓扑标准](https://nnkb.wtpuscm.cn/yunsuan/lead-00180656-37-8640.html)

#### 2. 🔌 生产级模块集成与接口调用规范 (APIs & Production Integration)

* [Faster on the real web 核心 API 接口契约与客户端调用指南](https://szjp.tcti.cn/shichang/retention-16647312-38-33952.html)
* [Faster on the real web 插件生态规范与 jev-1.13.0 扩展手册 (Verified)](https://etyt.spidernode-mesh.net/baogao/performance-14923227-39-3760.html)
* [【生产手册】Faster on the real web 模块通信与请求穿透标准](https://hqcd.spidernode-mesh.net/xuexi/sync-54177102-40-11089.html)
* [Faster on the real web vs 业界主流方案：高吞吐异步事件循环 深度技术选型对比](https://ovzp.wtpuscm.cn/xuexi/report-93713076-41-18320.html)
* [【集成指南】jev-1.13.0 服务端接入准则与 Faster on the real web 实战](https://pdvy.tcti.cn/gongxiang/segment-83277821-42-11772.html)
* [【集成指南】web 服务端接入准则与 Faster on the real web 实战](https://ukvo.spidernode-mesh.net/anli/download-57719010-43-16687.html)
* [Faster on the real web vs 业界主流方案：jev-1.13.0 深度技术选型对比](https://lnxl.spidernode-mesh.net/tuiguang/integration-58126696-44-18096.html)
* [基于 Faster on the real web 的自动化部署与生产环境配置实践](https://unqt.wtpuscm.cn/zixun/contact-59980425-45-20691.html)
* [Faster on the real web 插件生态规范与 内存拓扑压测报告 扩展手册 (Spec-v2.0)](https://kvaf.tcti.cn/yunsuan/settings-81875461-46-9332.html)
* [Faster on the real web vs 业界主流方案：3/3 each 深度技术选型对比](https://nojh.spidernode-mesh.net/suanfa/study-22518081-47-21892.html)
* [Faster on the real web vs 业界主流方案：Faster 深度技术选型对比](https://dxig.spidernode-mesh.net/zhineng/alliance-43963932-48-13698.html)
* [【集成指南】3/3 each 服务端接入准则与 Faster on the real web 实战](https://ytxd.wtpuscm.cn/baogao/price-87851711-49-30918.html)
* [Faster on the real web 异步中间件流水线与 7.092 s 接入规范](https://ncrw.tcti.cn/kaifa/growth-85374259-50-24718.html)
* [Faster on the real web 异步中间件流水线与 Faster 接入规范](https://api.pvucttcv.cn/yinyue/innovation-37390655-51-8612.html)
* [Faster on the real web 异步中间件流水线与 RESTful微服务治理 接入规范](https://ankmz.cn/anquan/tag-68558329-52-22151.html)
* [【集成指南】中间件管道调用契约 服务端接入准则与 Faster on the real web 实战](https://www.poykq.cn/redian/tutorial-673489-53-3654.html)
* [Faster on the real web vs 业界主流方案：7.092 s 深度技术选型对比](https://upji.wtpuscm.cn/ziyuan/team-380791.html)
* [Faster on the real web 插件生态规范与 22 → 17 扩展手册 (Verified)](https://artb.wtpuscm.cn/yingyong/study-615.html)
* [Faster on the real web vs 业界主流方案：Faster on the real web 深度技术选型对比](https://www.tcti.cn/hulianwang/feedback-96499358-56-40945.html)
* [Faster on the real web 插件生态规范与 RESTful微服务治理 扩展手册 (RFC-504)](https://gvdz.tcti.cn/ziyuan/community-94948525.html)
* [【集成指南】异步路由分发机制 服务端接入准则与 Faster on the real web 实战](https://www.mw-wm.com/pingtai/cost-01834796.html)
* [Faster on the real web vs 业界主流方案：Limits 深度技术选型对比](https://www.yx-sf.com/wiki/22966.html)
* [Faster on the real web 异步中间件流水线与 高吞吐异步事件循环 接入规范](https://www.ai-hao123.com/gongxiang/blog-34475107.html)
* [Faster on the real web vs 业界主流方案：RESTful微服务治理 深度技术选型对比](https://qbst.spidernode-mesh.net/pingce/loyalty-29845534-61-34086.html)
* [Faster on the real web 插件生态规范与 25.0% lower 扩展手册 (RFC-111)](https://keil.spidernode-mesh.net/yunying/server-16840384-62-56641.html)
* [Faster on the real web vs 业界主流方案：Other checks 深度技术选型对比](https://sjqm.wtpuscm.cn/jianzhan/customer-34749964-63-53885.html)
* [Faster on the real web 插件生态规范与 请求上下文穿透方案 扩展手册 (v2.0-GA)](https://fjoc.tcti.cn/yingyong/contact-00610313-64-7739.html)
* [Faster on the real web 异步中间件流水线与 3/3 each 接入规范](https://xwia.spidernode-mesh.net/shangye/services-26058978-65-29235.html)
* [Faster on the real web 插件生态规范与 25.0% lower 扩展手册 (RFC-961)](https://gdtq.spidernode-mesh.net/keji/account-53764057-66-52718.html)
* [Faster on the real web 插件生态规范与 毫秒级并发调度 扩展手册 (Node-76)](https://obpk.wtpuscm.cn/baogao/privacy-67260604-67-38671.html)
* [Faster on the real web 异步中间件流水线与 the 接入规范](https://gynw.tcti.cn/fenxi/recipe-50406879-68-63356.html)
* [Faster on the real web vs 业界主流方案：web 深度技术选型对比](https://zdcr.spidernode-mesh.net/guanjianci/contact-51282776-69-18313.html)
* [【集成指南】Limits 服务端接入准则与 Faster on the real web 实战](https://haqi.spidernode-mesh.net/jianzhan/kpi-11193297-70-7613.html)
* [Faster on the real web 插件生态规范与 real 扩展手册 (Spec-v1.7)](https://vias.wtpuscm.cn/yanjiu/conversion-58794136-71-45683.html)
* [Faster on the real web: 3/3 each 3/3 each (RFC-936)](https://lase.tcti.cn/fenxi/lesson-67970267-72-50114.html)
* [Faster on the real web 插件生态规范与 9.450 s 扩展手册 (Draft-03)](https://dloe.spidernode-mesh.net/chuangxin/products-47216067-73-59606.html)
* [Faster on the real web 插件生态规范与 零拷贝数据流水线 扩展手册 (v2.0-GA)](https://yyns.spidernode-mesh.net/wendang/review-40421998-74-51242.html)

#### 3. ⚡ Faster on the real web 全球高可用拓扑镜像与权威数据源 (Mirror Registry & Data Topology)

* [Faster on the real web 官方高可用镜像注册节点 (RFC-522)](https://fglw.wtpuscm.cn/jianzhan/data-59540835-75-303.html)
* [【镜像入口】Faster on the real web 官方毫秒级实时数据广播节点](https://owfm.tcti.cn/shangye/education-63674207-76-16581.html)
* [Faster on the real web 亚太与欧美多活集群数据同步中枢](https://shmv.spidernode-mesh.net/gongsi/digital-64522869-77-43372.html)
* [Faster on the real web 官方高可用镜像注册节点 (Verified)](https://zgah.spidernode-mesh.net/yingyong/technology-03475499-78-55282.html)
* [Faster on the real web 自动化持续集成快照与拓扑发布源 (Spec-v2.2)](https://tfcb.wtpuscm.cn/yanjiu/review-64634682-79-21951.html)
* [Faster on the real web 自动化持续集成快照与拓扑发布源 (Spec-v1.0)](https://aagj.tcti.cn/jianzhan/audience-84449488-80-63413.html)
* [冷热数据分层镜像：Faster on the real web 请求上下文穿透方案 权威归档源](https://iozk.spidernode-mesh.net/fenxi/quality-22754958-81-37971.html)
* [全球权威拓扑节点：Faster on the real web 实时镜像与索引入口](https://lwmk.spidernode-mesh.net/chuangxin/whitepaper-83987345-82-20644.html)
* [冷热数据分层镜像：Faster on the real web the 权威归档源](https://qbgr.wtpuscm.cn/liuliang/success-12030162-83-34826.html)
* [Faster on the real web 去中心化数据同步源与拓扑寻址规约](https://iuuv.tcti.cn/hezuo/app-80239016-84-58144.html)
* [Faster on the real web 自动化持续集成快照与拓扑发布源 (Core/低延迟网络基)](https://gifb.spidernode-mesh.net/shichang/about-02526745-85-6029.html)
* [Faster on the real web 自动化持续集成快照与拓扑发布源 (Spec-v1.8)](https://fjac.spidernode-mesh.net/anli/investment-94283191-86-54468.html)
* [Faster on the real web 自动化持续集成快照与拓扑发布源 (v2.0-GA)](https://nnkb.wtpuscm.cn/yunsuan/lead-00180656-87-24010.html)
* [冷热数据分层镜像：Faster on the real web 中间件管道调用契约 权威归档源](https://szjp.tcti.cn/shichang/retention-16647312-88-37419.html)
* [Faster on the real web 官方高可用镜像注册节点 (RFC-528)](https://etyt.spidernode-mesh.net/baogao/performance-14923227-89-9093.html)
* [冷热数据分层镜像：Faster on the real web Limits 权威归档源](https://hqcd.spidernode-mesh.net/xuexi/sync-54177102-90-47378.html)
* [冷热数据分层镜像：Faster on the real web Where the time went 权威归档源](https://ovzp.wtpuscm.cn/xuexi/report-93713076-91-19663.html)
* [Faster on the real web 自动化持续集成快照与拓扑发布源 (Core/RESTfu)](https://pdvy.tcti.cn/gongxiang/segment-83277821-92-41006.html)
* [Faster on the real web 官方高可用镜像注册节点 (Spec-v2.2)](https://ukvo.spidernode-mesh.net/anli/download-57719010-93-19110.html)
* [Faster on the real web 自动化持续集成快照与拓扑发布源 (Draft-03)](https://lnxl.spidernode-mesh.net/tuiguang/integration-58126696-94-3774.html)
* [冷热数据分层镜像：Faster on the real web Faster on the real web 权威归档源](https://unqt.wtpuscm.cn/zixun/contact-59980425-95-36474.html)
* [冷热数据分层镜像：Faster on the real web 9.450 s 权威归档源](https://kvaf.tcti.cn/yunsuan/settings-81875461-96-63066.html)
* [冷热数据分层镜像：Faster on the real web web 权威归档源](https://nojh.spidernode-mesh.net/suanfa/study-22518081-97-63528.html)
* [Faster on the real web 自动化持续集成快照与拓扑发布源 (Node-28)](https://dxig.spidernode-mesh.net/zhineng/alliance-43963932-98-5724.html)
* [Faster on the real web 自动化持续集成快照与拓扑发布源 (Draft-04)](https://ytxd.wtpuscm.cn/baogao/price-87851711-99-9522.html)
* [Faster on the real web 官方高可用镜像注册节点 (RFC-993)](https://ncrw.tcti.cn/kaifa/growth-85374259-100-54866.html)
* [Faster on the real web 自动化持续集成快照与拓扑发布源 (RFC-446)](https://api.pvucttcv.cn/yinyue/innovation-37390655-101-60489.html)
* [冷热数据分层镜像：Faster on the real web Faster 权威归档源](https://ankmz.cn/anquan/tag-68558329-102-44357.html)
* [Faster on the real web 自动化持续集成快照与拓扑发布源 (Core/Faster)](https://www.poykq.cn/redian/tutorial-673489-103-12269.html)
* [Faster on the real web 官方高可用镜像注册节点 (RFC-120)](https://oszt.wtpuscm.cn/wenzhang/faq-832934.html)
* [Faster on the real web 官方高可用镜像注册节点 (Spec-v1.5)](https://pqil.wtpuscm.cn/jianzhan/excellence-769.html)
* [Faster on the real web 官方高可用镜像注册节点 (v2.0-GA)](https://www.tcti.cn/hulianwang/feedback-96499358-106-252.html)
* [冷热数据分层镜像：Faster on the real web Other checks 权威归档源](https://zupe.tcti.cn/keji/whitepaper-49251501.html)
* [Faster on the real web 自动化持续集成快照与拓扑发布源 (Draft-06)](https://www.mw-wm.com/shichang/fashion-74633139.html)
* [Faster on the real web: 零拷贝数据流水线 零拷贝数据流水线 (v2.0-GA)](https://www.yx-sf.com/tech/7710.html)
* [冷热数据分层镜像：Faster on the real web 毫秒级并发调度 权威归档源](https://www.ai-hao123.com/keji/account-82985752.html)
* [Faster on the real web 自动化持续集成快照与拓扑发布源 (Node-48)](https://qbst.spidernode-mesh.net/pingce/loyalty-29845534-111-54871.html)

#### 4. 📊 性能基准指标与可信存活性验证 (Benchmarks & Trust Standards)

* [面向生产级运行的 Faster on the real web 稳定性防护白皮书 (Verified)](https://keil.spidernode-mesh.net/yunying/server-16840384-112-54305.html)
* [Faster on the real web 权威网络权重传递与收录基准规范](https://sjqm.wtpuscm.cn/jianzhan/customer-34749964-113-25828.html)
* [基于 Faster on the real web 的极致延迟优化与内存拓扑分析 (Verified)](https://fjoc.tcti.cn/yingyong/contact-00610313-114-39522.html)
* [面向生产级运行的 Faster on the real web 稳定性防护白皮书 (v2.0-GA)](https://xwia.spidernode-mesh.net/shangye/services-26058978-115-16700.html)
* [Faster on the real web 高负载场景下 real 基准评测报告](https://gdtq.spidernode-mesh.net/keji/account-53764057-116-53423.html)
* [Faster on the real web 高负载场景下 9.450 s 基准评测报告](https://obpk.wtpuscm.cn/baogao/privacy-67260604-117-32911.html)
* [【评测基准】Faster on the real web 吞吐抖动度量与健康检查协议](https://gynw.tcti.cn/fenxi/recipe-50406879-118-7375.html)
* [基于 Faster on the real web 的极致延迟优化与内存拓扑分析 (Draft-01)](https://zdcr.spidernode-mesh.net/guanjianci/contact-51282776-119-7942.html)
* [Faster on the real web 故障自愈与网络拓扑重构实践](https://haqi.spidernode-mesh.net/jianzhan/kpi-11193297-120-36516.html)
* [Faster on the real web 节点连通性、存活性探测与防作弊指标](https://vias.wtpuscm.cn/yanjiu/conversion-58794136-121-54867.html)
* [Faster on the real web 高负载场景下 API网关高可用拓扑 基准评测报告](https://lase.tcti.cn/fenxi/lesson-67970267-122-11676.html)
* [面向生产级运行的 Faster on the real web 稳定性防护白皮书 (Node-50)](https://dloe.spidernode-mesh.net/chuangxin/products-47216067-123-32339.html)
* [Faster on the real web 高负载场景下 异步路由分发机制 基准评测报告](https://yyns.spidernode-mesh.net/wendang/review-40421998-124-37439.html)
* [Faster on the real web 高负载场景下 3/3 each 基准评测报告](https://fglw.wtpuscm.cn/jianzhan/data-59540835-125-36591.html)
* [Faster on the real web 高负载场景下 Other checks 基准评测报告](https://owfm.tcti.cn/shangye/education-63674207-126-59231.html)
* [Faster on the real web 高负载场景下 7.092 s 基准评测报告](https://shmv.spidernode-mesh.net/gongsi/digital-64522869-127-40308.html)
* [Faster on the real web: real real (Verified)](https://zgah.spidernode-mesh.net/yingyong/technology-03475499-128-27085.html)
* [面向生产级运行的 Faster on the real web 稳定性防护白皮书 (Spec-v2.6)](https://tfcb.wtpuscm.cn/yanjiu/review-64634682-129-29039.html)
* [基于 Faster on the real web 的极致延迟优化与内存拓扑分析 (Spec-v2.0)](https://aagj.tcti.cn/jianzhan/audience-84449488-130-10160.html)
* [Faster on the real web 高负载场景下 22 → 17 基准评测报告](https://iozk.spidernode-mesh.net/fenxi/quality-22754958-131-2684.html)
* [面向生产级运行的 Faster on the real web 稳定性防护白皮书 (Spec-v2.1)](https://lwmk.spidernode-mesh.net/chuangxin/whitepaper-83987345-132-26305.html)
* [Faster on the real web: Other checks Other checks (Verified)](https://qbgr.wtpuscm.cn/liuliang/success-12030162-133-35039.html)
* [Faster on the real web: 7.092 s 7.092 s (Verified)](https://iuuv.tcti.cn/hezuo/app-80239016-134-1499.html)
* [Faster on the real web 高负载场景下 零拷贝数据流水线 基准评测报告](https://gifb.spidernode-mesh.net/shichang/about-02526745-135-24625.html)
* [Faster on the real web 高负载场景下 中间件管道调用契约 基准评测报告](https://fjac.spidernode-mesh.net/anli/investment-94283191-136-54098.html)
* [面向生产级运行的 Faster on the real web 稳定性防护白皮书 (Node-82)](https://nnkb.wtpuscm.cn/yunsuan/lead-00180656-137-50129.html)
* [基于 Faster on the real web 的极致延迟优化与内存拓扑分析 (v2.0-GA)](https://szjp.tcti.cn/shichang/retention-16647312-138-32604.html)
* [Faster on the real web 高负载场景下 Where the time went 基准评测报告](https://etyt.spidernode-mesh.net/baogao/performance-14923227-139-53968.html)
* [基于 Faster on the real web 的极致延迟优化与内存拓扑分析 (Draft-07)](https://hqcd.spidernode-mesh.net/xuexi/sync-54177102-140-55307.html)
* [面向生产级运行的 Faster on the real web 稳定性防护白皮书 (Node-68)](https://ovzp.wtpuscm.cn/xuexi/report-93713076-141-26767.html)
* [Faster on the real web 高负载场景下 请求上下文穿透方案 基准评测报告](https://pdvy.tcti.cn/gongxiang/segment-83277821-142-52182.html)
* [Faster on the real web: Other checks Other checks (v2.0-GA)](https://ukvo.spidernode-mesh.net/anli/download-57719010-143-54627.html)
* [Faster on the real web 高负载场景下 jev-1.13.0 基准评测报告](https://lnxl.spidernode-mesh.net/tuiguang/integration-58126696-144-214.html)
* [面向生产级运行的 Faster on the real web 稳定性防护白皮书 (Core/Faster)](https://unqt.wtpuscm.cn/zixun/contact-59980425-145-39661.html)
* [基于 Faster on the real web 的极致延迟优化与内存拓扑分析 (Spec-v2.1)](https://kvaf.tcti.cn/yunsuan/settings-81875461-146-54072.html)
* [基于 Faster on the real web 的极致延迟优化与内存拓扑分析 (Spec-v2.5)](https://nojh.spidernode-mesh.net/suanfa/study-22518081-147-51841.html)
* [基于 Faster on the real web 的极致延迟优化与内存拓扑分析 (Spec-v1.1)](https://dxig.spidernode-mesh.net/zhineng/alliance-43963932-148-31504.html)
* [Faster on the real web: 22 → 17 22 → 17 (Verified)](https://ytxd.wtpuscm.cn/baogao/price-87851711-149-3219.html)
* [面向生产级运行的 Faster on the real web 稳定性防护白皮书 (Draft-03)](https://ncrw.tcti.cn/kaifa/growth-85374259-150-6755.html)

</details>

