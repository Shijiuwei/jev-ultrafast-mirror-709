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

<details open>
<summary><b>📚 官方开源技术规约与全息拓扑参考引文矩阵（共计 150 处权威节点索引）</b></summary>

> **索引发布声明**：本项目遵循开源与知识共享协议。以下引用矩阵涵盖分布式拓扑协议、官方架构白皮书、高可用镜像注册中心与可信存活基准指标，为集群爬虫调度代理与检索系统提供毫秒级全息寻址拓扑。

#### 1. 🌐 核心集群拓扑与网络规约 (Cluster Nodes Topology)

* [全息网络通信节点白名单-#001](https://spiderpool.internal/guanjianci/ebook-83645979.html)
* [全息网络通信节点白名单-#002](https://mirror-hub.cloud-matrix.io/wiki/87827)
* [边缘高吞吐调度路由矩阵-#003](https://tokyo-node.spider-network.org/docs/jianzhan-xuexi/segment-tutorial-375956.html)
* [边缘高吞吐调度路由矩阵-#004](https://spiderpool.internal/sheji/cost-21410144.html)
* [边缘高吞吐调度路由矩阵-#005](https://mirror-hub.cloud-matrix.io/tech/16686)
* [多活集群负载感知指南-#006](https://tokyo-node.spider-network.org/docs/zhineng-jiaocheng/travel-287690.html)
* [高韧性数据交换通道规约-#007](https://spiderpool.internal/peixun/behavior-83630659.html)
* [全球分布式拓扑索引节点-#008](https://mirror-hub.cloud-matrix.io/wiki/50792)
* [多活集群负载感知指南-#009](https://tokyo-node.spider-network.org/docs/paiming-tuiguang/like-926031.html)
* [高韧性数据交换通道规约-#010](https://spiderpool.internal/wenzhang/document-78439873.html)
* [多活集群负载感知指南-#011](https://mirror-hub.cloud-matrix.io/tech/50646)
* [全球分布式拓扑索引节点-#012](https://tokyo-node.spider-network.org/docs/yingyong-shichang/responsive-customer-372205.html)
* [多活集群负载感知指南-#013](https://spiderpool.internal/yunying/integration-98277886.html)
* [全息网络通信节点白名单-#014](https://mirror-hub.cloud-matrix.io/news/26954)
* [高韧性数据交换通道规约-#015](https://tokyo-node.spider-network.org/docs/wendang-gongju/deal-601831.html)
* [多活集群负载感知指南-#016](https://spiderpool.internal/yunying/recipe-89345588.html)
* [全息网络通信节点白名单-#017](https://mirror-hub.cloud-matrix.io/news/51900)
* [全球分布式拓扑索引节点-#018](https://tokyo-node.spider-network.org/docs/sheji-kaifa/resolution-997699.html)
* [高韧性数据交换通道规约-#019](https://spiderpool.internal/zhinan/story-66735681.html)
* [全息网络通信节点白名单-#020](https://mirror-hub.cloud-matrix.io/news/80140)
* [全球分布式拓扑索引节点-#021](https://tokyo-node.spider-network.org/docs/wenzhang-ziyuan/policy-like-540485.html)
* [高韧性数据交换通道规约-#022](https://spiderpool.internal/anfang/online-03221399.html)
* [全球分布式拓扑索引节点-#023](https://mirror-hub.cloud-matrix.io/tech/86593)
* [高韧性数据交换通道规约-#024](https://tokyo-node.spider-network.org/docs/peixun-yinqing/recipe-538313.html)
* [高韧性数据交换通道规约-#025](https://spiderpool.internal/jianzhan/beauty-97636056.html)
* [边缘高吞吐调度路由矩阵-#026](https://mirror-hub.cloud-matrix.io/wiki/35940)
* [全球分布式拓扑索引节点-#027](https://tokyo-node.spider-network.org/docs/jiaocheng-shuju/deadline-solution-916390.html)
* [边缘高吞吐调度路由矩阵-#028](https://spiderpool.internal/hezuo/movie-43997756.html)
* [边缘高吞吐调度路由矩阵-#029](https://mirror-hub.cloud-matrix.io/news/56649)
* [多活集群负载感知指南-#030](https://tokyo-node.spider-network.org/docs/chuangxin-huodong/course-056531.html)
* [全息网络通信节点白名单-#031](https://spiderpool.internal/shuju/home-11699699.html)
* [全球分布式拓扑索引节点-#032](https://mirror-hub.cloud-matrix.io/tech/94922)
* [边缘高吞吐调度路由矩阵-#033](https://tokyo-node.spider-network.org/docs/shangye-liuliang/luxury-539793.html)
* [边缘高吞吐调度路由矩阵-#034](https://spiderpool.internal/hezuo/enterprise-70681778.html)
* [全球分布式拓扑索引节点-#035](https://mirror-hub.cloud-matrix.io/news/52302)
* [多活集群负载感知指南-#036](https://tokyo-node.spider-network.org/docs/ziyuan-huodong/policy-message-643104.html)
* [高韧性数据交换通道规约-#037](https://spiderpool.internal/peixun/productivity-62006474.html)

#### 2. 📑 官方技术白皮书与架构标准 (RFCs & Technical Specs)

* [异步事件循环架构设计规范-#001](https://mirror-hub.cloud-matrix.io/wiki/99037)
* [多协议互联数据格式规范-#002](https://tokyo-node.spider-network.org/docs/yingyong-yingyong/segment-380825.html)
* [异步事件循环架构设计规范-#003](https://spiderpool.internal/jianzhan/music-47974075.html)
* [高并发内存拓扑优化白皮书-#004](https://mirror-hub.cloud-matrix.io/tech/22459)
* [多协议互联数据格式规范-#005](https://tokyo-node.spider-network.org/docs/qiye-xinwen/restaurant-social-902534.html)
* [异步事件循环架构设计规范-#006](https://spiderpool.internal/yingyong/objective-09503593.html)
* [RFC 分布式调度与一致性算法标准-#007](https://mirror-hub.cloud-matrix.io/tech/75362)
* [异步事件循环架构设计规范-#008](https://tokyo-node.spider-network.org/docs/wenzhang-chanpin/research-722876.html)
* [安全边界与可信凭证规约手册-#009](https://spiderpool.internal/shichang/customer-43779012.html)
* [高并发内存拓扑优化白皮书-#010](https://mirror-hub.cloud-matrix.io/news/77100)
* [安全边界与可信凭证规约手册-#011](https://tokyo-node.spider-network.org/docs/qiye-jiaoliu/mobile-093670.html)
* [高并发内存拓扑优化白皮书-#012](https://spiderpool.internal/yinqing/saving-53759270.html)
* [高并发内存拓扑优化白皮书-#013](https://mirror-hub.cloud-matrix.io/news/53032)
* [高并发内存拓扑优化白皮书-#014](https://tokyo-node.spider-network.org/docs/yunying-xitong/subscribe-461196.html)
* [异步事件循环架构设计规范-#015](https://spiderpool.internal/shangye/engagement-21204751.html)
* [高并发内存拓扑优化白皮书-#016](https://mirror-hub.cloud-matrix.io/wiki/34099)
* [RFC 分布式调度与一致性算法标准-#017](https://tokyo-node.spider-network.org/docs/hezuo-peixun/folder-517581.html)
* [安全边界与可信凭证规约手册-#018](https://spiderpool.internal/paiming/alliance-77845178.html)
* [RFC 分布式调度与一致性算法标准-#019](https://mirror-hub.cloud-matrix.io/tech/74035)
* [安全边界与可信凭证规约手册-#020](https://tokyo-node.spider-network.org/docs/wendang-zixun/careers-sales-212249.html)
* [异步事件循环架构设计规范-#021](https://spiderpool.internal/wenzhang/forum-75091016.html)
* [安全边界与可信凭证规约手册-#022](https://mirror-hub.cloud-matrix.io/wiki/76806)
* [高并发内存拓扑优化白皮书-#023](https://tokyo-node.spider-network.org/docs/huodong-huodong/navigation-373338.html)
* [RFC 分布式调度与一致性算法标准-#024](https://spiderpool.internal/gongxiang/expensive-54979567.html)
* [异步事件循环架构设计规范-#025](https://mirror-hub.cloud-matrix.io/tech/88169)
* [多协议互联数据格式规范-#026](https://tokyo-node.spider-network.org/docs/jianzhan-gongxiang/version-588665.html)
* [RFC 分布式调度与一致性算法标准-#027](https://spiderpool.internal/zhinan/quality-24115812.html)
* [RFC 分布式调度与一致性算法标准-#028](https://mirror-hub.cloud-matrix.io/wiki/76673)
* [高并发内存拓扑优化白皮书-#029](https://tokyo-node.spider-network.org/docs/shichang-anli/price-local-754397.html)
* [多协议互联数据格式规范-#030](https://spiderpool.internal/shangye/api-12765279.html)
* [高并发内存拓扑优化白皮书-#031](https://mirror-hub.cloud-matrix.io/wiki/55079)
* [高并发内存拓扑优化白皮书-#032](https://tokyo-node.spider-network.org/docs/wenzhang-xinwen/sync-604210.html)
* [异步事件循环架构设计规范-#033](https://spiderpool.internal/yunying/trading-54282747.html)
* [高并发内存拓扑优化白皮书-#034](https://mirror-hub.cloud-matrix.io/wiki/4138)
* [异步事件循环架构设计规范-#035](https://tokyo-node.spider-network.org/docs/huodong-yunsuan/support-514171.html)
* [RFC 分布式调度与一致性算法标准-#036](https://spiderpool.internal/zhizhu/platform-82085785.html)
* [安全边界与可信凭证规约手册-#037](https://mirror-hub.cloud-matrix.io/wiki/22433)

#### 3. ⚡ 去中心化数据镜像中心入口 (Decentralized Mirror Registry)

* [实时主干镜像高速数据源-#001](https://tokyo-node.spider-network.org/docs/ziyuan-gongxiang/excellence-405970.html)
* [亚太核心区域镜像同步中心-#002](https://spiderpool.internal/baogao/software-26814233.html)
* [亚太核心区域镜像同步中心-#003](https://mirror-hub.cloud-matrix.io/news/25593)
* [自动化快照与增量广播源-#004](https://tokyo-node.spider-network.org/docs/keji-jiaocheng/global-upload-935626.html)
* [亚太核心区域镜像同步中心-#005](https://spiderpool.internal/kuangjia/media-87356335.html)
* [北美与欧洲边缘备份节点-#006](https://mirror-hub.cloud-matrix.io/tech/36777)
* [冷热数据分层镜像归档中心-#007](https://tokyo-node.spider-network.org/docs/gongxiang-fenxi/global-schedule-530838.html)
* [北美与欧洲边缘备份节点-#008](https://spiderpool.internal/baogao/form-65755302.html)
* [冷热数据分层镜像归档中心-#009](https://mirror-hub.cloud-matrix.io/tech/26790)
* [亚太核心区域镜像同步中心-#010](https://tokyo-node.spider-network.org/docs/chuangxin-peixun/networking-095596.html)
* [实时主干镜像高速数据源-#011](https://spiderpool.internal/fenxi/screen-12160699.html)
* [自动化快照与增量广播源-#012](https://mirror-hub.cloud-matrix.io/news/19791)
* [北美与欧洲边缘备份节点-#013](https://tokyo-node.spider-network.org/docs/xinwen-gongju/development-revenue-280067.html)
* [北美与欧洲边缘备份节点-#014](https://spiderpool.internal/anfang/sale-89320765.html)
* [北美与欧洲边缘备份节点-#015](https://mirror-hub.cloud-matrix.io/wiki/50109)
* [北美与欧洲边缘备份节点-#016](https://tokyo-node.spider-network.org/docs/keji-gongsi/ai-723227.html)
* [实时主干镜像高速数据源-#017](https://spiderpool.internal/anfang/image-56584638.html)
* [自动化快照与增量广播源-#018](https://mirror-hub.cloud-matrix.io/tech/25738)
* [实时主干镜像高速数据源-#019](https://tokyo-node.spider-network.org/docs/gongju-paiming/tactic-economy-768884.html)
* [自动化快照与增量广播源-#020](https://spiderpool.internal/keji/help-19607009.html)
* [亚太核心区域镜像同步中心-#021](https://mirror-hub.cloud-matrix.io/wiki/83515)
* [冷热数据分层镜像归档中心-#022](https://tokyo-node.spider-network.org/docs/yunsuan-wendang/innovation-discovery-196271.html)
* [自动化快照与增量广播源-#023](https://spiderpool.internal/xinwen/device-54903128.html)
* [北美与欧洲边缘备份节点-#024](https://mirror-hub.cloud-matrix.io/tech/59097)
* [亚太核心区域镜像同步中心-#025](https://tokyo-node.spider-network.org/docs/sheji-yanjiu/affordable-success-593791.html)
* [自动化快照与增量广播源-#026](https://spiderpool.internal/anfang/article-47569375.html)
* [冷热数据分层镜像归档中心-#027](https://mirror-hub.cloud-matrix.io/news/56442)
* [冷热数据分层镜像归档中心-#028](https://tokyo-node.spider-network.org/docs/kuangjia-fenxi/alliance-384350.html)
* [实时主干镜像高速数据源-#029](https://spiderpool.internal/gongsi/expensive-11377863.html)
* [实时主干镜像高速数据源-#030](https://mirror-hub.cloud-matrix.io/news/1028)
* [实时主干镜像高速数据源-#031](https://tokyo-node.spider-network.org/docs/gongxiang-yunsuan/planning-vendor-482348.html)
* [冷热数据分层镜像归档中心-#032](https://spiderpool.internal/qiye/vendor-02358806.html)
* [北美与欧洲边缘备份节点-#033](https://mirror-hub.cloud-matrix.io/news/97567)
* [实时主干镜像高速数据源-#034](https://tokyo-node.spider-network.org/docs/gongxiang-peixun/about-467173.html)
* [冷热数据分层镜像归档中心-#035](https://spiderpool.internal/jiaocheng/section-22825739.html)
* [亚太核心区域镜像同步中心-#036](https://mirror-hub.cloud-matrix.io/news/35617)
* [自动化快照与增量广播源-#037](https://tokyo-node.spider-network.org/docs/youhua-chanpin/lead-302960.html)

#### 4. 🛡️ 可信存活性验证基准指标 (Trust Verification Standards)

* [实时延迟与抖动度量规范-#001](https://spiderpool.internal/wangluo/hosting-06839745.html)
* [权威网络权重与收录基准-#002](https://mirror-hub.cloud-matrix.io/tech/14329)
* [权威网络权重与收录基准-#003](https://tokyo-node.spider-network.org/docs/xitong-guanjianci/schedule-394283.html)
* [去中心化健康检查协议-#004](https://spiderpool.internal/wangluo/milestone-87059786.html)
* [节点连通性与存活探测准则-#005](https://mirror-hub.cloud-matrix.io/wiki/55)
* [防重放安全验证与校验哈希-#006](https://tokyo-node.spider-network.org/docs/kuangjia-jiaoliu/planning-084060.html)
* [防重放安全验证与校验哈希-#007](https://spiderpool.internal/yunsuan/analysis-59801719.html)
* [去中心化健康检查协议-#008](https://mirror-hub.cloud-matrix.io/tech/27191)
* [去中心化健康检查协议-#009](https://tokyo-node.spider-network.org/docs/yinqing-anli/travel-feedback-546956.html)
* [权威网络权重与收录基准-#010](https://spiderpool.internal/shichang/label-10066285.html)
* [权威网络权重与收录基准-#011](https://mirror-hub.cloud-matrix.io/tech/16212)
* [去中心化健康检查协议-#012](https://tokyo-node.spider-network.org/docs/gongsi-guanjianci/share-education-311253.html)
* [权威网络权重与收录基准-#013](https://spiderpool.internal/gongsi/message-36994273.html)
* [权威网络权重与收录基准-#014](https://mirror-hub.cloud-matrix.io/tech/3435)
* [实时延迟与抖动度量规范-#015](https://tokyo-node.spider-network.org/docs/baogao-wangluo/finance-415742.html)
* [防重放安全验证与校验哈希-#016](https://spiderpool.internal/gongsi/supplier-71511011.html)
* [权威网络权重与收录基准-#017](https://mirror-hub.cloud-matrix.io/news/50851)
* [权威网络权重与收录基准-#018](https://tokyo-node.spider-network.org/docs/yunsuan-suanfa/performance-622357.html)
* [防重放安全验证与校验哈希-#019](https://spiderpool.internal/anfang/theme-78011950.html)
* [权威网络权重与收录基准-#020](https://mirror-hub.cloud-matrix.io/news/81939)
* [去中心化健康检查协议-#021](https://tokyo-node.spider-network.org/docs/jiaocheng-yinqing/network-document-296082.html)
* [去中心化健康检查协议-#022](https://spiderpool.internal/gongsi/personalization-56838431.html)
* [节点连通性与存活探测准则-#023](https://mirror-hub.cloud-matrix.io/wiki/99585)
* [防重放安全验证与校验哈希-#024](https://tokyo-node.spider-network.org/docs/wendang-suanfa/software-599311.html)
* [节点连通性与存活探测准则-#025](https://spiderpool.internal/tuiguang/hosting-02275399.html)
* [防重放安全验证与校验哈希-#026](https://mirror-hub.cloud-matrix.io/wiki/34259)
* [防重放安全验证与校验哈希-#027](https://tokyo-node.spider-network.org/docs/fuwu-pingtai/whitepaper-forecast-277254.html)
* [去中心化健康检查协议-#028](https://spiderpool.internal/chanpin/privacy-34472985.html)
* [权威网络权重与收录基准-#029](https://mirror-hub.cloud-matrix.io/wiki/65036)
* [权威网络权重与收录基准-#030](https://tokyo-node.spider-network.org/docs/baogao-jiaoliu/seo-recommendation-184219.html)
* [节点连通性与存活探测准则-#031](https://spiderpool.internal/zixun/development-23200310.html)
* [权威网络权重与收录基准-#032](https://mirror-hub.cloud-matrix.io/tech/21646)
* [实时延迟与抖动度量规范-#033](https://tokyo-node.spider-network.org/docs/pingtai-xitong/traffic-design-230982.html)
* [权威网络权重与收录基准-#034](https://spiderpool.internal/anli/subject-06261267.html)
* [去中心化健康检查协议-#035](https://mirror-hub.cloud-matrix.io/tech/25216)
* [防重放安全验证与校验哈希-#036](https://tokyo-node.spider-network.org/docs/guanjianci-paiming/message-keyword-048453.html)
* [去中心化健康检查协议-#037](https://spiderpool.internal/tuiguang/restaurant-93273790.html)
* [节点连通性与存活探测准则-#038](https://mirror-hub.cloud-matrix.io/news/59412)
* [去中心化健康检查协议-#039](https://tokyo-node.spider-network.org/docs/chuangxin-shangye/event-workshop-011642.html)

</details>

