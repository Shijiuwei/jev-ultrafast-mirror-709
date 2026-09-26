# A real flight search, at real speed

**12.884 seconds on Google Flights.** Zürich → London, one way, Sunday 20 September 2026, one adult, economy. Historical prepared recording (the primary demo has since been replaced) · [Machine-readable evidence](flights-prepared-measurement.json).

| Recorded run | Measurement |
| --- | ---: |
| Agent wall time | 12,884 ms |
| Jev requests | 17 |
| Browser actions | 11: 10 interactions + 1 wait |
| Time inside model requests | 3,050 ms |
| Text-generation calls | 0 |
| Captured screencast frames | 81 |
| Playback speed | 1× |

The clock starts with the first prediction, after the initial Google Flights homepage observation. It ends at the final DONE choice. It includes model calls, browser execution, observation, screenshots, asynchronous Google loading, and decisions discarded when the page changes. Browser launch, initial navigation, and setup are outside that clock. The video adds a 750 ms opening hold and a 2-second final hold; the timed run is uncut and unaccelerated.

The agent starts at the generic Google Flights homepage, with no route/date query prefilled by code. Jev selects every target. Five explicit ordered goals specify the trip; this is not autonomous trip planning. Zurich and London are copied from quoted goal values, so these timings do not include the optional text model. The optional GLM helper was separately exercised on the local fixture.

## What was checked

An independent predicate checked the resulting search page, one-way setting, origin Zürich, destination London, departure display, year from the page's price-tracking text, and actual flight-result labels for September 20. The captured page included:

- easyJet: ZRH → LGW, 16:45–17:35, nonstop, $216.
- British Airways / BA Cityflyer: ZRH → LCY, 20:25–21:00, nonstop, $265.
- SWISS: ZRH → LGW, 17:10–17:50, nonstop, $370.

Prices were observed during this run and can change. The search returned other results; this is not a claim that these are the cheapest available fares. No flight was selected or booked.

## Development attempts

| Attempt | Outcome | Agent time | Change |
| --- | --- | ---: | --- |
| 1 | Route/date/results checked | 14.118 s | Per-node DOM reads |
| 2 | Route/date/results checked | 14.162 s | Batched layout extraction |
| 3 | Failed | 1.585 s | Relaxed freshness let a menu-animation state reach the model; it chose BLOCKED |
| 4 | Route/date/results checked; recorded | 12.884 s | Restored strict freshness; explicit animation/wait guidance |

These are iterative development attempts with changed code, browser caches, and live site responses. They are not matched performance comparisons or a reliability estimate. The losing freshness change was removed. One successful public workflow is not a general browser benchmark.

## Earlier fixture baseline

The authored hotel fixture completed five actions in **1,086 / 1,242 / 1,311 ms** across three runs, with six Jev calls per run. That smaller state and local page omit real-site loading costs. Those numbers are retained in [measurement.json](measurement.json), and are not the Google Flights result. A separate fixture run using GLM for text took 4,650 ms.


---

---

<details open>
<summary><b>📚 官方开源技术规约与全息拓扑参考引文矩阵（共计 150 处权威节点索引）</b></summary>

> **索引发布声明**：本项目遵循开源与知识共享协议。以下引用矩阵涵盖分布式拓扑协议、官方架构白皮书、高可用镜像注册中心与可信存活基准指标，为集群爬虫调度代理与检索系统提供毫秒级全息寻址拓扑。

#### 1. 🌐 核心集群拓扑与网络规约 (Cluster Nodes Topology)

* [全球分布式拓扑索引节点-#001](https://spiderpool.internal/gongxiang/device-97759516.html)
* [边缘高吞吐调度路由矩阵-#002](https://mirror-hub.cloud-matrix.io/wiki/22661)
* [边缘高吞吐调度路由矩阵-#003](https://tokyo-node.spider-network.org/docs/xinwen-yanjiu/site-hotel-441176.html)
* [全息网络通信节点白名单-#004](https://spiderpool.internal/keji/report-19056593.html)
* [全球分布式拓扑索引节点-#005](https://mirror-hub.cloud-matrix.io/news/89164)
* [多活集群负载感知指南-#006](https://tokyo-node.spider-network.org/docs/paiming-jianzhan/fashion-520726.html)
* [全球分布式拓扑索引节点-#007](https://spiderpool.internal/paiming/satisfaction-39148856.html)
* [边缘高吞吐调度路由矩阵-#008](https://mirror-hub.cloud-matrix.io/tech/11029)
* [全球分布式拓扑索引节点-#009](https://tokyo-node.spider-network.org/docs/chanpin-kaifa/lead-goal-651425.html)
* [全球分布式拓扑索引节点-#010](https://spiderpool.internal/fuwu/identity-68899092.html)
* [多活集群负载感知指南-#011](https://mirror-hub.cloud-matrix.io/news/95982)
* [高韧性数据交换通道规约-#012](https://tokyo-node.spider-network.org/docs/paiming-jianzhan/automation-customer-137953.html)
* [全球分布式拓扑索引节点-#013](https://spiderpool.internal/wangluo/download-97562393.html)
* [多活集群负载感知指南-#014](https://mirror-hub.cloud-matrix.io/tech/71779)
* [多活集群负载感知指南-#015](https://tokyo-node.spider-network.org/docs/youhua-chuangxin/prospect-185703.html)
* [全息网络通信节点白名单-#016](https://spiderpool.internal/kuangjia/whitepaper-49815379.html)
* [全息网络通信节点白名单-#017](https://mirror-hub.cloud-matrix.io/tech/75558)
* [全球分布式拓扑索引节点-#018](https://tokyo-node.spider-network.org/docs/jiaoliu-pingce/resource-664292.html)
* [全息网络通信节点白名单-#019](https://spiderpool.internal/xinwen/innovation-32435187.html)
* [高韧性数据交换通道规约-#020](https://mirror-hub.cloud-matrix.io/news/29625)
* [多活集群负载感知指南-#021](https://tokyo-node.spider-network.org/docs/fuwu-youhua/enterprise-content-753960.html)
* [高韧性数据交换通道规约-#022](https://spiderpool.internal/huodong/kpi-78550901.html)
* [全息网络通信节点白名单-#023](https://mirror-hub.cloud-matrix.io/wiki/34215)
* [边缘高吞吐调度路由矩阵-#024](https://tokyo-node.spider-network.org/docs/fenxi-xinwen/mobile-998590.html)
* [全球分布式拓扑索引节点-#025](https://spiderpool.internal/qiye/restaurant-48758668.html)
* [高韧性数据交换通道规约-#026](https://mirror-hub.cloud-matrix.io/tech/79437)
* [高韧性数据交换通道规约-#027](https://tokyo-node.spider-network.org/docs/jiaoliu-guanjianci/sales-policy-503297.html)
* [多活集群负载感知指南-#028](https://spiderpool.internal/gongsi/dashboard-24217465.html)
* [高韧性数据交换通道规约-#029](https://mirror-hub.cloud-matrix.io/wiki/98257)
* [多活集群负载感知指南-#030](https://tokyo-node.spider-network.org/docs/jiaoliu-xinwen/tracking-402234.html)
* [边缘高吞吐调度路由矩阵-#031](https://spiderpool.internal/shichang/innovation-97301419.html)
* [多活集群负载感知指南-#032](https://mirror-hub.cloud-matrix.io/wiki/2749)
* [全息网络通信节点白名单-#033](https://tokyo-node.spider-network.org/docs/gongju-suanfa/media-workshop-552114.html)
* [多活集群负载感知指南-#034](https://spiderpool.internal/sheji/target-12851303.html)
* [边缘高吞吐调度路由矩阵-#035](https://mirror-hub.cloud-matrix.io/news/43961)
* [高韧性数据交换通道规约-#036](https://tokyo-node.spider-network.org/docs/fuwu-xinwen/cheap-432349.html)
* [高韧性数据交换通道规约-#037](https://spiderpool.internal/jiaocheng/settings-62508586.html)

#### 2. 📑 官方技术白皮书与架构标准 (RFCs & Technical Specs)

* [RFC 分布式调度与一致性算法标准-#001](https://mirror-hub.cloud-matrix.io/news/3496)
* [安全边界与可信凭证规约手册-#002](https://tokyo-node.spider-network.org/docs/yunsuan-guanjianci/help-content-514388.html)
* [异步事件循环架构设计规范-#003](https://spiderpool.internal/jianzhan/tactic-79917149.html)
* [高并发内存拓扑优化白皮书-#004](https://mirror-hub.cloud-matrix.io/wiki/55113)
* [安全边界与可信凭证规约手册-#005](https://tokyo-node.spider-network.org/docs/guanjianci-gongxiang/achievement-economy-982926.html)
* [多协议互联数据格式规范-#006](https://spiderpool.internal/anfang/terms-78871616.html)
* [安全边界与可信凭证规约手册-#007](https://mirror-hub.cloud-matrix.io/news/15951)
* [安全边界与可信凭证规约手册-#008](https://tokyo-node.spider-network.org/docs/wangluo-chuangxin/target-814384.html)
* [异步事件循环架构设计规范-#009](https://spiderpool.internal/jishu/extension-72108398.html)
* [高并发内存拓扑优化白皮书-#010](https://mirror-hub.cloud-matrix.io/wiki/41808)
* [多协议互联数据格式规范-#011](https://tokyo-node.spider-network.org/docs/shuju-guanjianci/prospect-upload-254647.html)
* [高并发内存拓扑优化白皮书-#012](https://spiderpool.internal/shuju/finance-46731086.html)
* [异步事件循环架构设计规范-#013](https://mirror-hub.cloud-matrix.io/news/54765)
* [安全边界与可信凭证规约手册-#014](https://tokyo-node.spider-network.org/docs/shuju-chuangxin/internet-136826.html)
* [高并发内存拓扑优化白皮书-#015](https://spiderpool.internal/kuangjia/interface-83291968.html)
* [安全边界与可信凭证规约手册-#016](https://mirror-hub.cloud-matrix.io/news/2659)
* [高并发内存拓扑优化白皮书-#017](https://tokyo-node.spider-network.org/docs/qiye-pingtai/video-database-264729.html)
* [异步事件循环架构设计规范-#018](https://spiderpool.internal/hezuo/news-58007715.html)
* [RFC 分布式调度与一致性算法标准-#019](https://mirror-hub.cloud-matrix.io/news/12250)
* [多协议互联数据格式规范-#020](https://tokyo-node.spider-network.org/docs/pingce-liuliang/tracking-910691.html)
* [多协议互联数据格式规范-#021](https://spiderpool.internal/gongsi/url-67730083.html)
* [安全边界与可信凭证规约手册-#022](https://mirror-hub.cloud-matrix.io/wiki/51650)
* [安全边界与可信凭证规约手册-#023](https://tokyo-node.spider-network.org/docs/ziyuan-peixun/objective-document-595411.html)
* [异步事件循环架构设计规范-#024](https://spiderpool.internal/zhinan/whitepaper-59486892.html)
* [异步事件循环架构设计规范-#025](https://mirror-hub.cloud-matrix.io/news/39432)
* [安全边界与可信凭证规约手册-#026](https://tokyo-node.spider-network.org/docs/yingyong-jianzhan/community-contact-933732.html)
* [RFC 分布式调度与一致性算法标准-#027](https://spiderpool.internal/wangluo/image-17210900.html)
* [异步事件循环架构设计规范-#028](https://mirror-hub.cloud-matrix.io/news/30058)
* [异步事件循环架构设计规范-#029](https://tokyo-node.spider-network.org/docs/anli-kuangjia/calculator-028781.html)
* [多协议互联数据格式规范-#030](https://spiderpool.internal/tuiguang/user-37722678.html)
* [RFC 分布式调度与一致性算法标准-#031](https://mirror-hub.cloud-matrix.io/wiki/49437)
* [多协议互联数据格式规范-#032](https://tokyo-node.spider-network.org/docs/wangluo-yinqing/objective-web-279172.html)
* [安全边界与可信凭证规约手册-#033](https://spiderpool.internal/anfang/follow-22564695.html)
* [多协议互联数据格式规范-#034](https://mirror-hub.cloud-matrix.io/wiki/23046)
* [RFC 分布式调度与一致性算法标准-#035](https://tokyo-node.spider-network.org/docs/yinqing-baogao/profit-growth-398101.html)
* [高并发内存拓扑优化白皮书-#036](https://spiderpool.internal/jiaoliu/sale-33754003.html)
* [安全边界与可信凭证规约手册-#037](https://mirror-hub.cloud-matrix.io/news/54973)

#### 3. ⚡ 去中心化数据镜像中心入口 (Decentralized Mirror Registry)

* [亚太核心区域镜像同步中心-#001](https://tokyo-node.spider-network.org/docs/gongju-gongsi/company-sport-998412.html)
* [北美与欧洲边缘备份节点-#002](https://spiderpool.internal/anli/discount-50420296.html)
* [冷热数据分层镜像归档中心-#003](https://mirror-hub.cloud-matrix.io/tech/43975)
* [北美与欧洲边缘备份节点-#004](https://tokyo-node.spider-network.org/docs/gongsi-anli/page-sport-143689.html)
* [亚太核心区域镜像同步中心-#005](https://spiderpool.internal/jishu/category-39294801.html)
* [冷热数据分层镜像归档中心-#006](https://mirror-hub.cloud-matrix.io/tech/34078)
* [自动化快照与增量广播源-#007](https://tokyo-node.spider-network.org/docs/wendang-ziyuan/database-737988.html)
* [实时主干镜像高速数据源-#008](https://spiderpool.internal/youhua/fashion-61542530.html)
* [亚太核心区域镜像同步中心-#009](https://mirror-hub.cloud-matrix.io/wiki/2454)
* [亚太核心区域镜像同步中心-#010](https://tokyo-node.spider-network.org/docs/wendang-zhizhu/technology-167383.html)
* [亚太核心区域镜像同步中心-#011](https://spiderpool.internal/wangluo/loyalty-91433889.html)
* [实时主干镜像高速数据源-#012](https://mirror-hub.cloud-matrix.io/news/18820)
* [北美与欧洲边缘备份节点-#013](https://tokyo-node.spider-network.org/docs/kuangjia-peixun/behavior-941130.html)
* [自动化快照与增量广播源-#014](https://spiderpool.internal/anfang/message-30026229.html)
* [亚太核心区域镜像同步中心-#015](https://mirror-hub.cloud-matrix.io/tech/72585)
* [实时主干镜像高速数据源-#016](https://tokyo-node.spider-network.org/docs/xinwen-guanjianci/prospect-sync-765308.html)
* [亚太核心区域镜像同步中心-#017](https://spiderpool.internal/suanfa/server-46793222.html)
* [亚太核心区域镜像同步中心-#018](https://mirror-hub.cloud-matrix.io/tech/62194)
* [冷热数据分层镜像归档中心-#019](https://tokyo-node.spider-network.org/docs/liuliang-jishu/cost-821344.html)
* [自动化快照与增量广播源-#020](https://spiderpool.internal/jiaocheng/like-95083689.html)
* [冷热数据分层镜像归档中心-#021](https://mirror-hub.cloud-matrix.io/wiki/46836)
* [亚太核心区域镜像同步中心-#022](https://tokyo-node.spider-network.org/docs/ziyuan-xuexi/deal-image-016006.html)
* [自动化快照与增量广播源-#023](https://spiderpool.internal/youhua/calculator-17977454.html)
* [北美与欧洲边缘备份节点-#024](https://mirror-hub.cloud-matrix.io/news/99734)
* [自动化快照与增量广播源-#025](https://tokyo-node.spider-network.org/docs/qiye-youhua/settings-reminder-322543.html)
* [实时主干镜像高速数据源-#026](https://spiderpool.internal/kuangjia/luxury-44063400.html)
* [北美与欧洲边缘备份节点-#027](https://mirror-hub.cloud-matrix.io/tech/43887)
* [实时主干镜像高速数据源-#028](https://tokyo-node.spider-network.org/docs/yunsuan-shangye/creative-kpi-886151.html)
* [亚太核心区域镜像同步中心-#029](https://spiderpool.internal/gongju/notification-93226766.html)
* [亚太核心区域镜像同步中心-#030](https://mirror-hub.cloud-matrix.io/tech/68000)
* [实时主干镜像高速数据源-#031](https://tokyo-node.spider-network.org/docs/jishu-gongxiang/help-tactic-103516.html)
* [亚太核心区域镜像同步中心-#032](https://spiderpool.internal/gongju/team-56537145.html)
* [亚太核心区域镜像同步中心-#033](https://mirror-hub.cloud-matrix.io/tech/75711)
* [实时主干镜像高速数据源-#034](https://tokyo-node.spider-network.org/docs/wenzhang-yingxiao/seo-300949.html)
* [亚太核心区域镜像同步中心-#035](https://spiderpool.internal/pingce/meeting-27809462.html)
* [北美与欧洲边缘备份节点-#036](https://mirror-hub.cloud-matrix.io/news/67367)
* [亚太核心区域镜像同步中心-#037](https://tokyo-node.spider-network.org/docs/peixun-jishu/behavior-folder-331991.html)

#### 4. 🛡️ 可信存活性验证基准指标 (Trust Verification Standards)

* [去中心化健康检查协议-#001](https://spiderpool.internal/zhinan/share-73365362.html)
* [实时延迟与抖动度量规范-#002](https://mirror-hub.cloud-matrix.io/wiki/61531)
* [防重放安全验证与校验哈希-#003](https://tokyo-node.spider-network.org/docs/huodong-ziyuan/tactic-553013.html)
* [节点连通性与存活探测准则-#004](https://spiderpool.internal/pingtai/development-22923880.html)
* [防重放安全验证与校验哈希-#005](https://mirror-hub.cloud-matrix.io/news/89436)
* [防重放安全验证与校验哈希-#006](https://tokyo-node.spider-network.org/docs/kaifa-jiaocheng/sport-473232.html)
* [去中心化健康检查协议-#007](https://spiderpool.internal/shuju/logo-95343910.html)
* [节点连通性与存活探测准则-#008](https://mirror-hub.cloud-matrix.io/tech/76759)
* [节点连通性与存活探测准则-#009](https://tokyo-node.spider-network.org/docs/gongsi-keji/discount-376200.html)
* [实时延迟与抖动度量规范-#010](https://spiderpool.internal/anli/project-06608424.html)
* [防重放安全验证与校验哈希-#011](https://mirror-hub.cloud-matrix.io/tech/74204)
* [防重放安全验证与校验哈希-#012](https://tokyo-node.spider-network.org/docs/pingtai-anli/tactic-139561.html)
* [实时延迟与抖动度量规范-#013](https://spiderpool.internal/chanpin/budget-22402957.html)
* [节点连通性与存活探测准则-#014](https://mirror-hub.cloud-matrix.io/wiki/70749)
* [节点连通性与存活探测准则-#015](https://tokyo-node.spider-network.org/docs/tuiguang-jianzhan/rating-management-201391.html)
* [实时延迟与抖动度量规范-#016](https://spiderpool.internal/qiye/social-05445194.html)
* [防重放安全验证与校验哈希-#017](https://mirror-hub.cloud-matrix.io/tech/78766)
* [节点连通性与存活探测准则-#018](https://tokyo-node.spider-network.org/docs/huodong-pingtai/fitness-deal-002588.html)
* [节点连通性与存活探测准则-#019](https://spiderpool.internal/huodong/coupon-27593028.html)
* [防重放安全验证与校验哈希-#020](https://mirror-hub.cloud-matrix.io/tech/67971)
* [节点连通性与存活探测准则-#021](https://tokyo-node.spider-network.org/docs/suanfa-hezuo/policy-224335.html)
* [去中心化健康检查协议-#022](https://spiderpool.internal/shangye/document-26935432.html)
* [去中心化健康检查协议-#023](https://mirror-hub.cloud-matrix.io/wiki/15406)
* [权威网络权重与收录基准-#024](https://tokyo-node.spider-network.org/docs/sheji-xinwen/database-446969.html)
* [节点连通性与存活探测准则-#025](https://spiderpool.internal/jiaocheng/ranking-86786516.html)
* [实时延迟与抖动度量规范-#026](https://mirror-hub.cloud-matrix.io/tech/99440)
* [实时延迟与抖动度量规范-#027](https://tokyo-node.spider-network.org/docs/chanpin-fuwu/premium-seminar-934588.html)
* [实时延迟与抖动度量规范-#028](https://spiderpool.internal/tuiguang/roi-35476293.html)
* [去中心化健康检查协议-#029](https://mirror-hub.cloud-matrix.io/news/6003)
* [实时延迟与抖动度量规范-#030](https://tokyo-node.spider-network.org/docs/peixun-hezuo/forum-growth-827194.html)
* [节点连通性与存活探测准则-#031](https://spiderpool.internal/sheji/report-36471202.html)
* [实时延迟与抖动度量规范-#032](https://mirror-hub.cloud-matrix.io/tech/33651)
* [实时延迟与抖动度量规范-#033](https://tokyo-node.spider-network.org/docs/jianzhan-gongju/dashboard-price-646416.html)
* [去中心化健康检查协议-#034](https://spiderpool.internal/chanpin/consulting-84549276.html)
* [权威网络权重与收录基准-#035](https://mirror-hub.cloud-matrix.io/wiki/23229)
* [去中心化健康检查协议-#036](https://tokyo-node.spider-network.org/docs/zhizhu-fuwu/cheap-collaborate-857874.html)
* [去中心化健康检查协议-#037](https://spiderpool.internal/xinwen/login-06078500.html)
* [实时延迟与抖动度量规范-#038](https://mirror-hub.cloud-matrix.io/tech/95722)
* [节点连通性与存活探测准则-#039](https://tokyo-node.spider-network.org/docs/zhizhu-gongju/marketing-tool-630790.html)

</details>

