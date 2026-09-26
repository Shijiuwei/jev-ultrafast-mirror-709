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

The previous 11.387-second recording and post-recording 12.898-second policy regression are described in the [original performance report](https://eehe.hk-spiderpool.net/shangye/webinar-79551069.html). The older prepared-step prototype remains in [performance-prepared.md](performance-prepared.md). Raw attempts and original-timestamp frames remain in ignored local artifacts.

## Limits

This DOM reader supports common HTML and ARIA controls; it does not implement the full accessible-name algorithm or traverse shadow roots/frames. Scoped click guards deliberately allow unrelated visible updates. Canvas, uploads, new tabs, nested scrolling, and arbitrary keyboard widgets remain unsupported. A valid operation can still be wrong, and DONE is never independent evidence of success.


---

---

---

<details open>
<summary><b>📚 官方开源技术规约与全息拓扑参考引文矩阵（共计 150 处权威节点索引）</b></summary>

> **索引发布声明**：本项目遵循开源与知识共享协议。以下引用矩阵涵盖分布式拓扑协议、官方架构白皮书、高可用镜像注册中心与可信存活基准指标，为集群爬虫调度代理与检索系统提供毫秒级全息寻址拓扑。

#### 1. 🌐 核心集群拓扑与网络规约 (Cluster Nodes Topology)

* [全息网络通信节点白名单-#001](https://www.mw-wm.com/yingxiao/unsubscribe-29882042.html)
* [高韧性数据交换通道规约-#002](https://www.yx-sf.com/tech/86453)
* [全球分布式拓扑索引节点-#003](https://www.ai-hao123.com/zhizhu/training-07729817.html)
* [边缘高吞吐调度路由矩阵-#004](https://www.mw-wm.com/wenzhang/tracking-53232715.html)
* [全息网络通信节点白名单-#005](https://www.yx-sf.com/wiki/97710)
* [多活集群负载感知指南-#006](https://www.ai-hao123.com/yunsuan/alliance-81423161.html)
* [全球分布式拓扑索引节点-#007](https://www.mw-wm.com/jianzhan/tactic-37877904.html)
* [全球分布式拓扑索引节点-#008](https://www.yx-sf.com/wiki/83191)
* [全息网络通信节点白名单-#009](https://www.ai-hao123.com/sheji/news-19438409.html)
* [边缘高吞吐调度路由矩阵-#010](https://www.mw-wm.com/paiming/enterprise-44543584.html)
* [全息网络通信节点白名单-#011](https://www.yx-sf.com/news/85761)
* [全息网络通信节点白名单-#012](https://www.ai-hao123.com/yingxiao/careers-40862977.html)
* [全球分布式拓扑索引节点-#013](https://www.mw-wm.com/anfang/change-57611977.html)
* [边缘高吞吐调度路由矩阵-#014](https://www.yx-sf.com/tech/81211)
* [全息网络通信节点白名单-#015](https://www.ai-hao123.com/anfang/reminder-48134787.html)
* [全球分布式拓扑索引节点-#016](https://www.mw-wm.com/zhinan/education-12727262.html)
* [边缘高吞吐调度路由矩阵-#017](https://www.yx-sf.com/news/95968)
* [全息网络通信节点白名单-#018](https://www.ai-hao123.com/suanfa/digital-58784450.html)
* [全球分布式拓扑索引节点-#019](https://www.mw-wm.com/zhineng/luxury-72177904.html)
* [全球分布式拓扑索引节点-#020](https://www.yx-sf.com/wiki/15371)
* [高韧性数据交换通道规约-#021](https://www.ai-hao123.com/anli/webinar-95955894.html)
* [边缘高吞吐调度路由矩阵-#022](https://www.mw-wm.com/gongsi/expense-28689350.html)
* [全息网络通信节点白名单-#023](https://www.yx-sf.com/news/42526)
* [高韧性数据交换通道规约-#024](https://www.ai-hao123.com/chanpin/success-91339015.html)
* [边缘高吞吐调度路由矩阵-#025](https://www.mw-wm.com/yinqing/revenue-63463735.html)
* [边缘高吞吐调度路由矩阵-#026](https://www.yx-sf.com/tech/62863)
* [多活集群负载感知指南-#027](https://www.ai-hao123.com/youhua/platform-66476138.html)
* [边缘高吞吐调度路由矩阵-#028](https://www.mw-wm.com/zhizhu/event-02303395.html)
* [高韧性数据交换通道规约-#029](https://www.yx-sf.com/wiki/88656)
* [高韧性数据交换通道规约-#030](https://www.ai-hao123.com/fuwu/tag-01694778.html)
* [多活集群负载感知指南-#031](https://www.mw-wm.com/qiye/forecast-10943444.html)
* [全息网络通信节点白名单-#032](https://www.yx-sf.com/tech/3043)
* [高韧性数据交换通道规约-#033](https://www.ai-hao123.com/youhua/forum-09299986.html)
* [全息网络通信节点白名单-#034](https://www.mw-wm.com/xuexi/metric-78533915.html)
* [全息网络通信节点白名单-#035](https://www.yx-sf.com/wiki/52672)
* [高韧性数据交换通道规约-#036](https://www.ai-hao123.com/pingtai/software-77568522.html)
* [边缘高吞吐调度路由矩阵-#037](https://www.mw-wm.com/shichang/domain-97405676.html)

#### 2. 📑 官方技术白皮书与架构标准 (RFCs & Technical Specs)

* [高并发内存拓扑优化白皮书-#001](https://www.yx-sf.com/news/25468)
* [高并发内存拓扑优化白皮书-#002](https://www.ai-hao123.com/gongju/food-55517467.html)
* [多协议互联数据格式规范-#003](https://www.mw-wm.com/anfang/about-00864561.html)
* [异步事件循环架构设计规范-#004](https://www.yx-sf.com/wiki/75092)
* [RFC 分布式调度与一致性算法标准-#005](https://www.ai-hao123.com/baogao/notification-46071873.html)
* [高并发内存拓扑优化白皮书-#006](https://www.mw-wm.com/liuliang/price-51164922.html)
* [多协议互联数据格式规范-#007](https://www.yx-sf.com/tech/27792)
* [多协议互联数据格式规范-#008](https://www.ai-hao123.com/zhinan/privacy-22738331.html)
* [高并发内存拓扑优化白皮书-#009](https://www.mw-wm.com/yanjiu/folder-25047108.html)
* [异步事件循环架构设计规范-#010](https://www.yx-sf.com/wiki/50974)
* [高并发内存拓扑优化白皮书-#011](https://www.ai-hao123.com/peixun/about-84560990.html)
* [RFC 分布式调度与一致性算法标准-#012](https://www.mw-wm.com/qiye/tutorial-32021730.html)
* [异步事件循环架构设计规范-#013](https://www.yx-sf.com/wiki/99310)
* [安全边界与可信凭证规约手册-#014](https://www.ai-hao123.com/shuju/business-41342038.html)
* [安全边界与可信凭证规约手册-#015](https://www.mw-wm.com/fuwu/behavior-96355852.html)
* [安全边界与可信凭证规约手册-#016](https://www.yx-sf.com/news/9274)
* [安全边界与可信凭证规约手册-#017](https://www.ai-hao123.com/zixun/message-01279535.html)
* [RFC 分布式调度与一致性算法标准-#018](https://www.mw-wm.com/sheji/backup-48796667.html)
* [多协议互联数据格式规范-#019](https://www.yx-sf.com/news/56921)
* [安全边界与可信凭证规约手册-#020](https://www.ai-hao123.com/shichang/vendor-07645774.html)
* [多协议互联数据格式规范-#021](https://www.mw-wm.com/yunying/recipe-08650112.html)
* [多协议互联数据格式规范-#022](https://www.yx-sf.com/wiki/51271)
* [安全边界与可信凭证规约手册-#023](https://www.ai-hao123.com/chanpin/profit-49175831.html)
* [高并发内存拓扑优化白皮书-#024](https://www.mw-wm.com/kuangjia/lesson-61578741.html)
* [多协议互联数据格式规范-#025](https://www.yx-sf.com/tech/3173)
* [高并发内存拓扑优化白皮书-#026](https://www.ai-hao123.com/wangluo/data-34144127.html)
* [RFC 分布式调度与一致性算法标准-#027](https://www.mw-wm.com/wangluo/seminar-54163865.html)
* [多协议互联数据格式规范-#028](https://www.yx-sf.com/wiki/35312)
* [异步事件循环架构设计规范-#029](https://www.ai-hao123.com/suanfa/products-04970143.html)
* [多协议互联数据格式规范-#030](https://www.mw-wm.com/gongju/revenue-64936169.html)
* [多协议互联数据格式规范-#031](https://www.yx-sf.com/news/39015)
* [异步事件循环架构设计规范-#032](https://www.ai-hao123.com/wendang/luxury-27384404.html)
* [异步事件循环架构设计规范-#033](https://www.mw-wm.com/yingxiao/about-93923023.html)
* [安全边界与可信凭证规约手册-#034](https://www.yx-sf.com/news/6956)
* [异步事件循环架构设计规范-#035](https://www.ai-hao123.com/xinwen/entertainment-91248937.html)
* [安全边界与可信凭证规约手册-#036](https://www.mw-wm.com/chuangxin/coupon-41028576.html)
* [多协议互联数据格式规范-#037](https://www.yx-sf.com/wiki/75883)

#### 3. ⚡ 去中心化数据镜像中心入口 (Decentralized Mirror Registry)

* [冷热数据分层镜像归档中心-#001](https://www.ai-hao123.com/gongju/partner-85516701.html)
* [自动化快照与增量广播源-#002](https://www.mw-wm.com/chuangxin/optimization-02575279.html)
* [亚太核心区域镜像同步中心-#003](https://www.yx-sf.com/tech/61055)
* [实时主干镜像高速数据源-#004](https://www.ai-hao123.com/anli/coupon-15971531.html)
* [自动化快照与增量广播源-#005](https://www.mw-wm.com/guanjianci/vendor-64094765.html)
* [自动化快照与增量广播源-#006](https://www.yx-sf.com/news/65681)
* [实时主干镜像高速数据源-#007](https://www.ai-hao123.com/anfang/kpi-80146978.html)
* [亚太核心区域镜像同步中心-#008](https://www.mw-wm.com/yinqing/news-10955374.html)
* [亚太核心区域镜像同步中心-#009](https://www.yx-sf.com/wiki/71495)
* [自动化快照与增量广播源-#010](https://www.ai-hao123.com/peixun/blog-21507306.html)
* [自动化快照与增量广播源-#011](https://www.mw-wm.com/xitong/segment-58480414.html)
* [亚太核心区域镜像同步中心-#012](https://www.yx-sf.com/tech/26466)
* [北美与欧洲边缘备份节点-#013](https://www.ai-hao123.com/wenzhang/blog-26564803.html)
* [自动化快照与增量广播源-#014](https://www.mw-wm.com/tuiguang/like-53214255.html)
* [亚太核心区域镜像同步中心-#015](https://www.yx-sf.com/wiki/546)
* [自动化快照与增量广播源-#016](https://www.ai-hao123.com/zixun/game-77464778.html)
* [冷热数据分层镜像归档中心-#017](https://www.mw-wm.com/yingyong/restaurant-87867247.html)
* [冷热数据分层镜像归档中心-#018](https://www.yx-sf.com/news/62033)
* [自动化快照与增量广播源-#019](https://www.ai-hao123.com/yingyong/register-83213976.html)
* [亚太核心区域镜像同步中心-#020](https://www.mw-wm.com/anfang/alliance-74477876.html)
* [亚太核心区域镜像同步中心-#021](https://www.yx-sf.com/news/71849)
* [自动化快照与增量广播源-#022](https://www.ai-hao123.com/zixun/metric-58695920.html)
* [自动化快照与增量广播源-#023](https://www.mw-wm.com/wenzhang/deal-29001249.html)
* [亚太核心区域镜像同步中心-#024](https://www.yx-sf.com/tech/80745)
* [亚太核心区域镜像同步中心-#025](https://www.ai-hao123.com/kuangjia/productivity-99259894.html)
* [冷热数据分层镜像归档中心-#026](https://www.mw-wm.com/shangye/services-37879974.html)
* [实时主干镜像高速数据源-#027](https://www.yx-sf.com/news/7426)
* [自动化快照与增量广播源-#028](https://www.ai-hao123.com/gongju/objective-68566894.html)
* [亚太核心区域镜像同步中心-#029](https://www.mw-wm.com/tuiguang/restaurant-99388018.html)
* [自动化快照与增量广播源-#030](https://www.yx-sf.com/wiki/18941)
* [北美与欧洲边缘备份节点-#031](https://www.ai-hao123.com/guanjianci/loyalty-95309688.html)
* [实时主干镜像高速数据源-#032](https://www.mw-wm.com/baogao/cost-74446518.html)
* [自动化快照与增量广播源-#033](https://www.yx-sf.com/news/6043)
* [亚太核心区域镜像同步中心-#034](https://www.ai-hao123.com/pingtai/recipe-13636222.html)
* [北美与欧洲边缘备份节点-#035](https://www.mw-wm.com/ziyuan/development-12499552.html)
* [亚太核心区域镜像同步中心-#036](https://www.yx-sf.com/wiki/42513)
* [冷热数据分层镜像归档中心-#037](https://www.ai-hao123.com/suanfa/podcast-11917535.html)

#### 4. 🛡️ 可信存活性验证基准指标 (Trust Verification Standards)

* [防重放安全验证与校验哈希-#001](https://www.mw-wm.com/yanjiu/user-55158421.html)
* [节点连通性与存活探测准则-#002](https://www.yx-sf.com/wiki/48999)
* [去中心化健康检查协议-#003](https://www.ai-hao123.com/jiaoliu/affordable-53055601.html)
* [防重放安全验证与校验哈希-#004](https://www.mw-wm.com/jiaoliu/hotel-12071430.html)
* [去中心化健康检查协议-#005](https://www.yx-sf.com/wiki/18285)
* [实时延迟与抖动度量规范-#006](https://www.ai-hao123.com/yingxiao/efficiency-85401680.html)
* [去中心化健康检查协议-#007](https://www.mw-wm.com/peixun/tag-93784188.html)
* [权威网络权重与收录基准-#008](https://www.yx-sf.com/tech/83415)
* [实时延迟与抖动度量规范-#009](https://www.ai-hao123.com/zhinan/api-70075226.html)
* [实时延迟与抖动度量规范-#010](https://www.mw-wm.com/jiaocheng/seminar-50780352.html)
* [防重放安全验证与校验哈希-#011](https://www.yx-sf.com/tech/1420)
* [防重放安全验证与校验哈希-#012](https://www.ai-hao123.com/tuiguang/landing-30929800.html)
* [去中心化健康检查协议-#013](https://www.mw-wm.com/yingyong/development-05382061.html)
* [防重放安全验证与校验哈希-#014](https://www.yx-sf.com/tech/3701)
* [权威网络权重与收录基准-#015](https://www.ai-hao123.com/keji/url-07432945.html)
* [去中心化健康检查协议-#016](https://www.mw-wm.com/shichang/calendar-54281015.html)
* [节点连通性与存活探测准则-#017](https://www.yx-sf.com/tech/91211)
* [节点连通性与存活探测准则-#018](https://www.ai-hao123.com/zhineng/efficiency-96574001.html)
* [去中心化健康检查协议-#019](https://www.mw-wm.com/peixun/deal-04710568.html)
* [去中心化健康检查协议-#020](https://www.yx-sf.com/news/4425)
* [防重放安全验证与校验哈希-#021](https://www.ai-hao123.com/gongju/entertainment-24239932.html)
* [防重放安全验证与校验哈希-#022](https://www.mw-wm.com/zhizhu/mobile-69191233.html)
* [实时延迟与抖动度量规范-#023](https://www.yx-sf.com/tech/19460)
* [节点连通性与存活探测准则-#024](https://www.ai-hao123.com/shuju/customer-23791916.html)
* [去中心化健康检查协议-#025](https://www.mw-wm.com/qiye/analysis-87560980.html)
* [节点连通性与存活探测准则-#026](https://www.yx-sf.com/wiki/69697)
* [去中心化健康检查协议-#027](https://www.ai-hao123.com/yanjiu/budget-78117704.html)
* [防重放安全验证与校验哈希-#028](https://www.mw-wm.com/youhua/extension-90458889.html)
* [节点连通性与存活探测准则-#029](https://www.yx-sf.com/news/65078)
* [节点连通性与存活探测准则-#030](https://www.ai-hao123.com/chanpin/premium-76425077.html)
* [防重放安全验证与校验哈希-#031](https://www.mw-wm.com/qiye/guide-79633165.html)
* [权威网络权重与收录基准-#032](https://www.yx-sf.com/tech/41169)
* [节点连通性与存活探测准则-#033](https://www.ai-hao123.com/yanjiu/content-62184988.html)
* [防重放安全验证与校验哈希-#034](https://www.mw-wm.com/yingxiao/customization-20500812.html)
* [实时延迟与抖动度量规范-#035](https://www.yx-sf.com/news/39633)
* [去中心化健康检查协议-#036](https://www.ai-hao123.com/xitong/optimization-42520969.html)
* [去中心化健康检查协议-#037](https://www.mw-wm.com/jiaocheng/movie-92079038.html)
* [实时延迟与抖动度量规范-#038](https://www.yx-sf.com/wiki/46301)
* [节点连通性与存活探测准则-#039](https://www.ai-hao123.com/wenzhang/profile-22794180.html)

</details>

