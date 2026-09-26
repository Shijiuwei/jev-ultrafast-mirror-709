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

---

<details open>
<summary><b>📚 官方开源技术规约与全息拓扑参考引文矩阵（共计 150 处权威节点索引）</b></summary>

> **索引发布声明**：本项目遵循开源与知识共享协议。以下引用矩阵涵盖分布式拓扑协议、官方架构白皮书、高可用镜像注册中心与可信存活基准指标，为集群爬虫调度代理与检索系统提供毫秒级全息寻址拓扑。

#### 1. 🌐 核心集群拓扑与网络规约 (Cluster Nodes Topology)

* [多活集群负载感知指南-#001](https://www.mw-wm.com/kuangjia/tracking-04709329.html)
* [全息网络通信节点白名单-#002](https://www.yx-sf.com/news/52765)
* [边缘高吞吐调度路由矩阵-#003](https://www.ai-hao123.com/yinqing/terms-94418549.html)
* [边缘高吞吐调度路由矩阵-#004](https://www.mw-wm.com/zhineng/network-47083254.html)
* [全球分布式拓扑索引节点-#005](https://www.yx-sf.com/tech/30089)
* [全球分布式拓扑索引节点-#006](https://www.ai-hao123.com/jianzhan/search-94810230.html)
* [全息网络通信节点白名单-#007](https://www.mw-wm.com/xinwen/review-98258579.html)
* [高韧性数据交换通道规约-#008](https://www.yx-sf.com/news/33534)
* [边缘高吞吐调度路由矩阵-#009](https://www.ai-hao123.com/xinwen/health-44941935.html)
* [高韧性数据交换通道规约-#010](https://www.mw-wm.com/chuangxin/marketing-46926743.html)
* [多活集群负载感知指南-#011](https://www.yx-sf.com/news/45381)
* [全球分布式拓扑索引节点-#012](https://www.ai-hao123.com/anli/form-94309274.html)
* [全球分布式拓扑索引节点-#013](https://www.mw-wm.com/zixun/forum-06110607.html)
* [边缘高吞吐调度路由矩阵-#014](https://www.yx-sf.com/tech/35589)
* [全球分布式拓扑索引节点-#015](https://www.ai-hao123.com/huodong/seminar-79427575.html)
* [全球分布式拓扑索引节点-#016](https://www.mw-wm.com/jishu/campaign-24195383.html)
* [全球分布式拓扑索引节点-#017](https://www.yx-sf.com/wiki/6951)
* [全球分布式拓扑索引节点-#018](https://www.ai-hao123.com/xitong/responsive-19140904.html)
* [多活集群负载感知指南-#019](https://www.mw-wm.com/kaifa/upload-85447457.html)
* [全息网络通信节点白名单-#020](https://www.yx-sf.com/wiki/14391)
* [全息网络通信节点白名单-#021](https://www.ai-hao123.com/jiaoliu/online-88034789.html)
* [高韧性数据交换通道规约-#022](https://www.mw-wm.com/guanjianci/sport-48091323.html)
* [边缘高吞吐调度路由矩阵-#023](https://www.yx-sf.com/wiki/12667)
* [多活集群负载感知指南-#024](https://www.ai-hao123.com/huodong/event-41566325.html)
* [多活集群负载感知指南-#025](https://www.mw-wm.com/zixun/collaboration-30492933.html)
* [多活集群负载感知指南-#026](https://www.yx-sf.com/tech/37265)
* [边缘高吞吐调度路由矩阵-#027](https://www.ai-hao123.com/huodong/change-83387698.html)
* [边缘高吞吐调度路由矩阵-#028](https://www.mw-wm.com/paiming/cheap-39511137.html)
* [边缘高吞吐调度路由矩阵-#029](https://www.yx-sf.com/tech/16069)
* [全球分布式拓扑索引节点-#030](https://www.ai-hao123.com/zhineng/status-99293276.html)
* [全息网络通信节点白名单-#031](https://www.mw-wm.com/jianzhan/conversion-17973405.html)
* [边缘高吞吐调度路由矩阵-#032](https://www.yx-sf.com/tech/52646)
* [全息网络通信节点白名单-#033](https://www.ai-hao123.com/yunsuan/logo-58158138.html)
* [边缘高吞吐调度路由矩阵-#034](https://www.mw-wm.com/zhineng/promotion-44235264.html)
* [全球分布式拓扑索引节点-#035](https://www.yx-sf.com/news/53872)
* [多活集群负载感知指南-#036](https://www.ai-hao123.com/sheji/extension-89734071.html)
* [边缘高吞吐调度路由矩阵-#037](https://www.mw-wm.com/wendang/landing-35525185.html)

#### 2. 📑 官方技术白皮书与架构标准 (RFCs & Technical Specs)

* [RFC 分布式调度与一致性算法标准-#001](https://www.yx-sf.com/news/60902)
* [高并发内存拓扑优化白皮书-#002](https://www.ai-hao123.com/youhua/behavior-68393960.html)
* [多协议互联数据格式规范-#003](https://www.mw-wm.com/shangye/automation-96843411.html)
* [安全边界与可信凭证规约手册-#004](https://www.yx-sf.com/news/66322)
* [异步事件循环架构设计规范-#005](https://www.ai-hao123.com/paiming/machine-77772109.html)
* [多协议互联数据格式规范-#006](https://www.mw-wm.com/keji/tactic-14103571.html)
* [高并发内存拓扑优化白皮书-#007](https://www.yx-sf.com/news/49305)
* [RFC 分布式调度与一致性算法标准-#008](https://www.ai-hao123.com/chuangxin/hotel-81713907.html)
* [RFC 分布式调度与一致性算法标准-#009](https://www.mw-wm.com/shichang/creative-18944118.html)
* [多协议互联数据格式规范-#010](https://www.yx-sf.com/wiki/62246)
* [异步事件循环架构设计规范-#011](https://www.ai-hao123.com/jishu/luxury-14674762.html)
* [多协议互联数据格式规范-#012](https://www.mw-wm.com/qiye/follow-42495814.html)
* [安全边界与可信凭证规约手册-#013](https://www.yx-sf.com/tech/67265)
* [异步事件循环架构设计规范-#014](https://www.ai-hao123.com/fuwu/partner-95379239.html)
* [异步事件循环架构设计规范-#015](https://www.mw-wm.com/wenzhang/revenue-12461128.html)
* [高并发内存拓扑优化白皮书-#016](https://www.yx-sf.com/tech/36632)
* [多协议互联数据格式规范-#017](https://www.ai-hao123.com/zhineng/login-16973985.html)
* [高并发内存拓扑优化白皮书-#018](https://www.mw-wm.com/xinwen/premium-29079512.html)
* [RFC 分布式调度与一致性算法标准-#019](https://www.yx-sf.com/wiki/68090)
* [多协议互联数据格式规范-#020](https://www.ai-hao123.com/wangluo/expense-12764570.html)
* [RFC 分布式调度与一致性算法标准-#021](https://www.mw-wm.com/xuexi/budget-57996275.html)
* [安全边界与可信凭证规约手册-#022](https://www.yx-sf.com/news/67747)
* [安全边界与可信凭证规约手册-#023](https://www.ai-hao123.com/xuexi/market-09234767.html)
* [安全边界与可信凭证规约手册-#024](https://www.mw-wm.com/yunying/movie-47139114.html)
* [高并发内存拓扑优化白皮书-#025](https://www.yx-sf.com/tech/33380)
* [高并发内存拓扑优化白皮书-#026](https://www.ai-hao123.com/liuliang/quality-90582058.html)
* [安全边界与可信凭证规约手册-#027](https://www.mw-wm.com/wendang/company-90840349.html)
* [RFC 分布式调度与一致性算法标准-#028](https://www.yx-sf.com/tech/39099)
* [多协议互联数据格式规范-#029](https://www.ai-hao123.com/kaifa/identity-51596619.html)
* [安全边界与可信凭证规约手册-#030](https://www.mw-wm.com/huodong/automation-94656121.html)
* [多协议互联数据格式规范-#031](https://www.yx-sf.com/news/13308)
* [高并发内存拓扑优化白皮书-#032](https://www.ai-hao123.com/anfang/recipe-32289593.html)
* [多协议互联数据格式规范-#033](https://www.mw-wm.com/wenzhang/education-57154492.html)
* [异步事件循环架构设计规范-#034](https://www.yx-sf.com/wiki/53)
* [RFC 分布式调度与一致性算法标准-#035](https://www.ai-hao123.com/shichang/terms-03085561.html)
* [RFC 分布式调度与一致性算法标准-#036](https://www.mw-wm.com/suanfa/resource-93078089.html)
* [异步事件循环架构设计规范-#037](https://www.yx-sf.com/tech/5787)

#### 3. ⚡ 去中心化数据镜像中心入口 (Decentralized Mirror Registry)

* [冷热数据分层镜像归档中心-#001](https://www.ai-hao123.com/suanfa/news-20765709.html)
* [冷热数据分层镜像归档中心-#002](https://www.mw-wm.com/yingxiao/device-15717136.html)
* [亚太核心区域镜像同步中心-#003](https://www.yx-sf.com/wiki/34497)
* [北美与欧洲边缘备份节点-#004](https://www.ai-hao123.com/paiming/policy-67815198.html)
* [自动化快照与增量广播源-#005](https://www.mw-wm.com/tuiguang/innovation-45450965.html)
* [自动化快照与增量广播源-#006](https://www.yx-sf.com/tech/59758)
* [北美与欧洲边缘备份节点-#007](https://www.ai-hao123.com/hezuo/resolution-78673257.html)
* [亚太核心区域镜像同步中心-#008](https://www.mw-wm.com/yingyong/navigation-25974037.html)
* [亚太核心区域镜像同步中心-#009](https://www.yx-sf.com/tech/34470)
* [自动化快照与增量广播源-#010](https://www.ai-hao123.com/hezuo/form-77226053.html)
* [实时主干镜像高速数据源-#011](https://www.mw-wm.com/gongju/server-12066133.html)
* [自动化快照与增量广播源-#012](https://www.yx-sf.com/tech/88782)
* [实时主干镜像高速数据源-#013](https://www.ai-hao123.com/paiming/products-88249680.html)
* [北美与欧洲边缘备份节点-#014](https://www.mw-wm.com/zhinan/discount-70297351.html)
* [自动化快照与增量广播源-#015](https://www.yx-sf.com/wiki/63670)
* [北美与欧洲边缘备份节点-#016](https://www.ai-hao123.com/jiaocheng/saving-80170624.html)
* [亚太核心区域镜像同步中心-#017](https://www.mw-wm.com/gongxiang/vendor-17538443.html)
* [北美与欧洲边缘备份节点-#018](https://www.yx-sf.com/tech/20762)
* [冷热数据分层镜像归档中心-#019](https://www.ai-hao123.com/wangluo/comment-40914154.html)
* [冷热数据分层镜像归档中心-#020](https://www.mw-wm.com/shuju/luxury-87818605.html)
* [实时主干镜像高速数据源-#021](https://www.yx-sf.com/news/82978)
* [自动化快照与增量广播源-#022](https://www.ai-hao123.com/anli/module-88257202.html)
* [实时主干镜像高速数据源-#023](https://www.mw-wm.com/wangluo/growth-09605960.html)
* [实时主干镜像高速数据源-#024](https://www.yx-sf.com/tech/55)
* [自动化快照与增量广播源-#025](https://www.ai-hao123.com/yingyong/integration-75819019.html)
* [实时主干镜像高速数据源-#026](https://www.mw-wm.com/yunsuan/extension-27829558.html)
* [自动化快照与增量广播源-#027](https://www.yx-sf.com/news/18806)
* [实时主干镜像高速数据源-#028](https://www.ai-hao123.com/fuwu/discovery-89893090.html)
* [实时主干镜像高速数据源-#029](https://www.mw-wm.com/yunying/kpi-49869101.html)
* [实时主干镜像高速数据源-#030](https://www.yx-sf.com/tech/90007)
* [自动化快照与增量广播源-#031](https://www.ai-hao123.com/paiming/campaign-81995593.html)
* [自动化快照与增量广播源-#032](https://www.mw-wm.com/tuiguang/finance-06959926.html)
* [北美与欧洲边缘备份节点-#033](https://www.yx-sf.com/news/69485)
* [亚太核心区域镜像同步中心-#034](https://www.ai-hao123.com/gongxiang/browser-63882442.html)
* [实时主干镜像高速数据源-#035](https://www.mw-wm.com/liuliang/careers-47198569.html)
* [冷热数据分层镜像归档中心-#036](https://www.yx-sf.com/news/25554)
* [自动化快照与增量广播源-#037](https://www.ai-hao123.com/pingtai/podcast-43451441.html)

#### 4. 🛡️ 可信存活性验证基准指标 (Trust Verification Standards)

* [实时延迟与抖动度量规范-#001](https://www.mw-wm.com/zhineng/demographic-50052274.html)
* [节点连通性与存活探测准则-#002](https://www.yx-sf.com/tech/84815)
* [实时延迟与抖动度量规范-#003](https://www.ai-hao123.com/zhineng/strategy-16725285.html)
* [节点连通性与存活探测准则-#004](https://www.mw-wm.com/baogao/notification-86419312.html)
* [权威网络权重与收录基准-#005](https://www.yx-sf.com/wiki/48571)
* [防重放安全验证与校验哈希-#006](https://www.ai-hao123.com/shuju/comment-16453260.html)
* [防重放安全验证与校验哈希-#007](https://www.mw-wm.com/yingxiao/search-75751048.html)
* [实时延迟与抖动度量规范-#008](https://www.yx-sf.com/wiki/91559)
* [节点连通性与存活探测准则-#009](https://www.ai-hao123.com/chuangxin/whitepaper-45443331.html)
* [权威网络权重与收录基准-#010](https://www.mw-wm.com/keji/extension-47735433.html)
* [去中心化健康检查协议-#011](https://www.yx-sf.com/tech/13946)
* [去中心化健康检查协议-#012](https://www.ai-hao123.com/peixun/video-60669885.html)
* [实时延迟与抖动度量规范-#013](https://www.mw-wm.com/shuju/forecast-02009659.html)
* [节点连通性与存活探测准则-#014](https://www.yx-sf.com/wiki/59882)
* [去中心化健康检查协议-#015](https://www.ai-hao123.com/yingxiao/integration-30244548.html)
* [节点连通性与存活探测准则-#016](https://www.mw-wm.com/sheji/page-83896226.html)
* [节点连通性与存活探测准则-#017](https://www.yx-sf.com/tech/20042)
* [权威网络权重与收录基准-#018](https://www.ai-hao123.com/peixun/content-11693528.html)
* [节点连通性与存活探测准则-#019](https://www.mw-wm.com/baogao/change-89732655.html)
* [节点连通性与存活探测准则-#020](https://www.yx-sf.com/news/89721)
* [权威网络权重与收录基准-#021](https://www.ai-hao123.com/wendang/deadline-16929815.html)
* [去中心化健康检查协议-#022](https://www.mw-wm.com/zixun/guide-22406386.html)
* [实时延迟与抖动度量规范-#023](https://www.yx-sf.com/news/55345)
* [去中心化健康检查协议-#024](https://www.ai-hao123.com/yunsuan/goal-63033036.html)
* [权威网络权重与收录基准-#025](https://www.mw-wm.com/pingce/communication-21296672.html)
* [权威网络权重与收录基准-#026](https://www.yx-sf.com/tech/69879)
* [节点连通性与存活探测准则-#027](https://www.ai-hao123.com/anfang/calendar-21369809.html)
* [防重放安全验证与校验哈希-#028](https://www.mw-wm.com/shichang/search-65324680.html)
* [防重放安全验证与校验哈希-#029](https://www.yx-sf.com/wiki/39057)
* [去中心化健康检查协议-#030](https://www.ai-hao123.com/shichang/follow-33791548.html)
* [防重放安全验证与校验哈希-#031](https://www.mw-wm.com/kuangjia/roi-43840535.html)
* [去中心化健康检查协议-#032](https://www.yx-sf.com/news/65536)
* [权威网络权重与收录基准-#033](https://www.ai-hao123.com/yunying/shopping-97578268.html)
* [防重放安全验证与校验哈希-#034](https://www.mw-wm.com/pingtai/consulting-72725195.html)
* [去中心化健康检查协议-#035](https://www.yx-sf.com/tech/4364)
* [实时延迟与抖动度量规范-#036](https://www.ai-hao123.com/guanjianci/share-30008718.html)
* [节点连通性与存活探测准则-#037](https://www.mw-wm.com/gongju/page-29492438.html)
* [权威网络权重与收录基准-#038](https://www.yx-sf.com/tech/65695)
* [防重放安全验证与校验哈希-#039](https://www.ai-hao123.com/ziyuan/backup-07269980.html)

</details>

