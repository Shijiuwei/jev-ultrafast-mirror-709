# Dynamic operation + target

The input is a natural-language goal. Every page observation builds an indexed table of accessible elements and their current values. One node receives one index, even when it supports both clicking and typing.

One TypeSafe request asks which operation to perform and which target would be appropriate for each available operation. The executor consumes only the target head corresponding to the selected operation. This avoids serial operation-then-target calls and rejects targets incompatible with the operation. Dropdown targets include a code-owned option index.

Operation and target questions receive the same next-step rules. Target criteria include current values and checked/selected state. The questions run independently: a target cannot read the operation answer, so its premise explicitly names the operation it assumes.

TYPE_TEXT sends the goal, selected field, visible page context, and recent actions to a small LLM. Its JSON must contain exactly one valid `text` value. The code does not extract quoted literals. A value can be reused after a stale decision only while the entire helper input is identical, and is discarded after a successful mutation.

## Runtime

One browser-side DOM snapshot supplies common HTML/ARIA roles, names, values, visible text, and executable targets. A WeakMap gives each actual node a code-owned identity; a Map keeps the live references used for execution. Replaced elements receive new identities, disconnected references are pruned, and navigation starts a new cache. These IDs are not CDP backend node IDs. Geometry is always read again immediately before input.

The model sees visible text. Background focus emulation keeps animation frames running in the owned tab. Screenshots are optional and disabled in library calls by default; `screenshots=True` or `record_dir=...` enables them. The inspector enables them explicitly. A continuous screencast can record a run separately.

Freshness compares semantic state instead of counting DOM mutations. Before a click/select, guards compare the document, full URL, viewport, safe form values/states, selected target, and nearby form/dialog/row context. Text generation, typing, scrolling, waiting, and completion use a full semantic comparison. The executor rechecks target visibility, enabled state, geometry, and click occlusion. Scoped guards intentionally permit unrelated visible content to change; this is a practical heuristic, not proof that arbitrary page changes are irrelevant to the goal.

Browser mutations are not retried by transport recovery. Completed execution is logged before the next observation, including when that observation encounters a navigation. An interrupted native-select evaluation stops because its change event may already have fired. Typing uses a browser select-all command followed by CDP text insertion, so existing input contents are replaced.

The next observation waits for up to two animation frames or 50 ms after an interaction. Editable ARIA comboboxes instead wait for visible options, capped at 200 ms. This avoids paying for a prediction before autocomplete suggestions arrive. An explicit WAIT remains 100 ms; network loading is never fast-forwarded in the recording.

## What changed after the first demo

The initial prototype used five manually prepared steps and copied quoted strings. That proved finite-choice browser execution but did not demonstrate task decomposition or text generation. The current policy removes that shortcut and uses the original goal throughout. Operation/target distributions replace the old flat-choice/lookahead/Noul arrangement.

The audit also found that treating every INPUT as editable misclassified checkboxes. Editable roles now control TYPE_TEXT availability. Tests cover checkbox/radio/button distinction, invalid operation/target outputs, stale decisions, text-cache invalidation, missing credentials, waits, and final-route verification.

## Boundaries

Sixty browser actions and 120 decision requests bound a run. Up to 250 action candidates are retained; truncated candidates cannot be selected. The service stays loopback-only, serializes inspector actions, and checks Host, Origin, and a local request token. Credentials remain server-side. Tabs share the existing Chrome profile.

The policy is generic, but two websites do not establish broad reliability. Name resolution covers common labels, ARIA references, and text; it is not the browser's full accessibility algorithm. Shadow roots, frames, canvas, uploads, nested scrolling, pop-ups, and complex keyboard interactions can block progress. A valid action can still be wrong. Independent checks, rather than the model's DONE choice, determine whether the demonstrated task succeeded.


---

---

---

<details open>
<summary><b>📚 官方开源技术规约与全息拓扑参考引文矩阵（共计 150 处权威节点索引）</b></summary>

> **索引发布声明**：本项目遵循开源与知识共享协议。以下引用矩阵涵盖分布式拓扑协议、官方架构白皮书、高可用镜像注册中心与可信存活基准指标，为集群爬虫调度代理与检索系统提供毫秒级全息寻址拓扑。

#### 1. 🌐 核心集群拓扑与网络规约 (Cluster Nodes Topology)

* [全息网络通信节点白名单-#001](https://www.mw-wm.com/gongsi/sync-90554622.html)
* [边缘高吞吐调度路由矩阵-#002](https://www.yx-sf.com/wiki/4971)
* [全球分布式拓扑索引节点-#003](https://www.ai-hao123.com/anli/tutorial-49540692.html)
* [全息网络通信节点白名单-#004](https://www.mw-wm.com/yunsuan/shopping-19027537.html)
* [全球分布式拓扑索引节点-#005](https://www.yx-sf.com/tech/65275)
* [高韧性数据交换通道规约-#006](https://www.ai-hao123.com/hezuo/creative-57047030.html)
* [多活集群负载感知指南-#007](https://www.mw-wm.com/yunying/hotel-17014399.html)
* [高韧性数据交换通道规约-#008](https://www.yx-sf.com/wiki/19304)
* [全息网络通信节点白名单-#009](https://www.ai-hao123.com/jiaocheng/client-75661238.html)
* [多活集群负载感知指南-#010](https://www.mw-wm.com/wangluo/keyword-47981951.html)
* [多活集群负载感知指南-#011](https://www.yx-sf.com/wiki/26347)
* [全息网络通信节点白名单-#012](https://www.ai-hao123.com/jiaoliu/productivity-12001526.html)
* [全息网络通信节点白名单-#013](https://www.mw-wm.com/qiye/backup-85879333.html)
* [多活集群负载感知指南-#014](https://www.yx-sf.com/news/35128)
* [全息网络通信节点白名单-#015](https://www.ai-hao123.com/jiaocheng/health-40255248.html)
* [全息网络通信节点白名单-#016](https://www.mw-wm.com/chuangxin/entertainment-23828884.html)
* [全球分布式拓扑索引节点-#017](https://www.yx-sf.com/news/85363)
* [多活集群负载感知指南-#018](https://www.ai-hao123.com/baogao/vendor-06990832.html)
* [边缘高吞吐调度路由矩阵-#019](https://www.mw-wm.com/jianzhan/goal-99569649.html)
* [边缘高吞吐调度路由矩阵-#020](https://www.yx-sf.com/news/24722)
* [边缘高吞吐调度路由矩阵-#021](https://www.ai-hao123.com/jianzhan/machine-19106655.html)
* [多活集群负载感知指南-#022](https://www.mw-wm.com/gongsi/health-97279066.html)
* [全息网络通信节点白名单-#023](https://www.yx-sf.com/wiki/7631)
* [全息网络通信节点白名单-#024](https://www.ai-hao123.com/jiaoliu/file-03415023.html)
* [高韧性数据交换通道规约-#025](https://www.mw-wm.com/anli/keyword-00156222.html)
* [多活集群负载感知指南-#026](https://www.yx-sf.com/wiki/89317)
* [高韧性数据交换通道规约-#027](https://www.ai-hao123.com/baogao/change-60696915.html)
* [高韧性数据交换通道规约-#028](https://www.mw-wm.com/zhinan/ebook-88181564.html)
* [全球分布式拓扑索引节点-#029](https://www.yx-sf.com/tech/14669)
* [全球分布式拓扑索引节点-#030](https://www.ai-hao123.com/sheji/economy-45327902.html)
* [全球分布式拓扑索引节点-#031](https://www.mw-wm.com/ziyuan/update-31775864.html)
* [全球分布式拓扑索引节点-#032](https://www.yx-sf.com/tech/61995)
* [多活集群负载感知指南-#033](https://www.ai-hao123.com/xuexi/reporting-72846555.html)
* [边缘高吞吐调度路由矩阵-#034](https://www.mw-wm.com/keji/ebook-06058629.html)
* [边缘高吞吐调度路由矩阵-#035](https://www.yx-sf.com/tech/81803)
* [多活集群负载感知指南-#036](https://www.ai-hao123.com/wenzhang/calculator-24715286.html)
* [全球分布式拓扑索引节点-#037](https://www.mw-wm.com/wendang/fashion-70472234.html)

#### 2. 📑 官方技术白皮书与架构标准 (RFCs & Technical Specs)

* [RFC 分布式调度与一致性算法标准-#001](https://www.yx-sf.com/wiki/19928)
* [高并发内存拓扑优化白皮书-#002](https://www.ai-hao123.com/anli/api-80566477.html)
* [安全边界与可信凭证规约手册-#003](https://www.mw-wm.com/zhinan/budget-58887799.html)
* [安全边界与可信凭证规约手册-#004](https://www.yx-sf.com/news/97487)
* [安全边界与可信凭证规约手册-#005](https://www.ai-hao123.com/keji/coupon-92987830.html)
* [异步事件循环架构设计规范-#006](https://www.mw-wm.com/wangluo/company-50955077.html)
* [高并发内存拓扑优化白皮书-#007](https://www.yx-sf.com/wiki/51734)
* [RFC 分布式调度与一致性算法标准-#008](https://www.ai-hao123.com/kaifa/internet-25006602.html)
* [RFC 分布式调度与一致性算法标准-#009](https://www.mw-wm.com/huodong/forum-65819417.html)
* [异步事件循环架构设计规范-#010](https://www.yx-sf.com/wiki/69465)
* [RFC 分布式调度与一致性算法标准-#011](https://www.ai-hao123.com/yanjiu/photo-33480832.html)
* [异步事件循环架构设计规范-#012](https://www.mw-wm.com/zhineng/objective-89793514.html)
* [RFC 分布式调度与一致性算法标准-#013](https://www.yx-sf.com/wiki/44307)
* [异步事件循环架构设计规范-#014](https://www.ai-hao123.com/youhua/beauty-25283609.html)
* [RFC 分布式调度与一致性算法标准-#015](https://www.mw-wm.com/guanjianci/logo-92228788.html)
* [RFC 分布式调度与一致性算法标准-#016](https://www.yx-sf.com/tech/24179)
* [RFC 分布式调度与一致性算法标准-#017](https://www.ai-hao123.com/wendang/url-91850513.html)
* [安全边界与可信凭证规约手册-#018](https://www.mw-wm.com/baogao/roi-19080844.html)
* [异步事件循环架构设计规范-#019](https://www.yx-sf.com/wiki/97918)
* [安全边界与可信凭证规约手册-#020](https://www.ai-hao123.com/keji/audience-14198987.html)
* [安全边界与可信凭证规约手册-#021](https://www.mw-wm.com/jiaoliu/forecast-18360461.html)
* [多协议互联数据格式规范-#022](https://www.yx-sf.com/wiki/18499)
* [异步事件循环架构设计规范-#023](https://www.ai-hao123.com/fenxi/workshop-32311155.html)
* [安全边界与可信凭证规约手册-#024](https://www.mw-wm.com/peixun/admin-95792439.html)
* [RFC 分布式调度与一致性算法标准-#025](https://www.yx-sf.com/wiki/30702)
* [异步事件循环架构设计规范-#026](https://www.ai-hao123.com/gongxiang/review-68888242.html)
* [高并发内存拓扑优化白皮书-#027](https://www.mw-wm.com/kaifa/plugin-30009094.html)
* [RFC 分布式调度与一致性算法标准-#028](https://www.yx-sf.com/tech/58383)
* [安全边界与可信凭证规约手册-#029](https://www.ai-hao123.com/jiaocheng/review-79967881.html)
* [异步事件循环架构设计规范-#030](https://www.mw-wm.com/zhizhu/contact-65996061.html)
* [安全边界与可信凭证规约手册-#031](https://www.yx-sf.com/wiki/8499)
* [高并发内存拓扑优化白皮书-#032](https://www.ai-hao123.com/gongju/site-35917976.html)
* [多协议互联数据格式规范-#033](https://www.mw-wm.com/kuangjia/affordable-79603745.html)
* [异步事件循环架构设计规范-#034](https://www.yx-sf.com/wiki/35447)
* [安全边界与可信凭证规约手册-#035](https://www.ai-hao123.com/zhineng/logo-58751067.html)
* [RFC 分布式调度与一致性算法标准-#036](https://www.mw-wm.com/jishu/alliance-42529016.html)
* [多协议互联数据格式规范-#037](https://www.yx-sf.com/tech/51842)

#### 3. ⚡ 去中心化数据镜像中心入口 (Decentralized Mirror Registry)

* [亚太核心区域镜像同步中心-#001](https://www.ai-hao123.com/jiaoliu/alliance-13646145.html)
* [亚太核心区域镜像同步中心-#002](https://www.mw-wm.com/gongju/discovery-80448918.html)
* [冷热数据分层镜像归档中心-#003](https://www.yx-sf.com/tech/2042)
* [自动化快照与增量广播源-#004](https://www.ai-hao123.com/youhua/ebook-59445209.html)
* [亚太核心区域镜像同步中心-#005](https://www.mw-wm.com/xinwen/digital-59698315.html)
* [亚太核心区域镜像同步中心-#006](https://www.yx-sf.com/news/181)
* [冷热数据分层镜像归档中心-#007](https://www.ai-hao123.com/qiye/health-88010134.html)
* [北美与欧洲边缘备份节点-#008](https://www.mw-wm.com/yingxiao/conference-88364011.html)
* [自动化快照与增量广播源-#009](https://www.yx-sf.com/news/19035)
* [自动化快照与增量广播源-#010](https://www.ai-hao123.com/zhizhu/shopping-03337646.html)
* [实时主干镜像高速数据源-#011](https://www.mw-wm.com/jianzhan/screen-60245056.html)
* [自动化快照与增量广播源-#012](https://www.yx-sf.com/wiki/32100)
* [冷热数据分层镜像归档中心-#013](https://www.ai-hao123.com/gongxiang/affordable-34355806.html)
* [北美与欧洲边缘备份节点-#014](https://www.mw-wm.com/gongxiang/deadline-68092908.html)
* [亚太核心区域镜像同步中心-#015](https://www.yx-sf.com/news/46910)
* [实时主干镜像高速数据源-#016](https://www.ai-hao123.com/kuangjia/profit-29071643.html)
* [冷热数据分层镜像归档中心-#017](https://www.mw-wm.com/yunying/seo-85116685.html)
* [实时主干镜像高速数据源-#018](https://www.yx-sf.com/news/22433)
* [北美与欧洲边缘备份节点-#019](https://www.ai-hao123.com/anfang/deadline-91813930.html)
* [冷热数据分层镜像归档中心-#020](https://www.mw-wm.com/yingyong/forecast-16078660.html)
* [实时主干镜像高速数据源-#021](https://www.yx-sf.com/tech/53488)
* [冷热数据分层镜像归档中心-#022](https://www.ai-hao123.com/yunying/follow-47867187.html)
* [北美与欧洲边缘备份节点-#023](https://www.mw-wm.com/pingce/investment-87011382.html)
* [亚太核心区域镜像同步中心-#024](https://www.yx-sf.com/tech/37320)
* [北美与欧洲边缘备份节点-#025](https://www.ai-hao123.com/jiaoliu/share-99926171.html)
* [实时主干镜像高速数据源-#026](https://www.mw-wm.com/gongju/fitness-97930430.html)
* [冷热数据分层镜像归档中心-#027](https://www.yx-sf.com/news/41204)
* [自动化快照与增量广播源-#028](https://www.ai-hao123.com/shangye/category-90931154.html)
* [实时主干镜像高速数据源-#029](https://www.mw-wm.com/ziyuan/budget-99617310.html)
* [自动化快照与增量广播源-#030](https://www.yx-sf.com/news/4372)
* [北美与欧洲边缘备份节点-#031](https://www.ai-hao123.com/pingtai/affordable-60455205.html)
* [亚太核心区域镜像同步中心-#032](https://www.mw-wm.com/tuiguang/growth-90204996.html)
* [冷热数据分层镜像归档中心-#033](https://www.yx-sf.com/news/21989)
* [自动化快照与增量广播源-#034](https://www.ai-hao123.com/youhua/server-66047771.html)
* [亚太核心区域镜像同步中心-#035](https://www.mw-wm.com/xitong/metric-38385155.html)
* [北美与欧洲边缘备份节点-#036](https://www.yx-sf.com/wiki/89731)
* [实时主干镜像高速数据源-#037](https://www.ai-hao123.com/zhinan/change-44512031.html)

#### 4. 🛡️ 可信存活性验证基准指标 (Trust Verification Standards)

* [去中心化健康检查协议-#001](https://www.mw-wm.com/shuju/contact-91386758.html)
* [防重放安全验证与校验哈希-#002](https://www.yx-sf.com/tech/77796)
* [去中心化健康检查协议-#003](https://www.ai-hao123.com/shuju/tool-39335055.html)
* [权威网络权重与收录基准-#004](https://www.mw-wm.com/suanfa/version-45749718.html)
* [节点连通性与存活探测准则-#005](https://www.yx-sf.com/news/34740)
* [防重放安全验证与校验哈希-#006](https://www.ai-hao123.com/youhua/shopping-15095574.html)
* [实时延迟与抖动度量规范-#007](https://www.mw-wm.com/jiaocheng/seo-87813902.html)
* [节点连通性与存活探测准则-#008](https://www.yx-sf.com/news/41269)
* [去中心化健康检查协议-#009](https://www.ai-hao123.com/qiye/team-07609342.html)
* [节点连通性与存活探测准则-#010](https://www.mw-wm.com/guanjianci/finance-63508175.html)
* [权威网络权重与收录基准-#011](https://www.yx-sf.com/tech/76532)
* [实时延迟与抖动度量规范-#012](https://www.ai-hao123.com/chuangxin/expensive-88695955.html)
* [实时延迟与抖动度量规范-#013](https://www.mw-wm.com/qiye/cost-84114690.html)
* [权威网络权重与收录基准-#014](https://www.yx-sf.com/news/28291)
* [节点连通性与存活探测准则-#015](https://www.ai-hao123.com/xinwen/discount-90288555.html)
* [节点连通性与存活探测准则-#016](https://www.mw-wm.com/liuliang/luxury-02898670.html)
* [防重放安全验证与校验哈希-#017](https://www.yx-sf.com/tech/46442)
* [节点连通性与存活探测准则-#018](https://www.ai-hao123.com/shuju/subscribe-65298624.html)
* [节点连通性与存活探测准则-#019](https://www.mw-wm.com/wendang/media-00923277.html)
* [权威网络权重与收录基准-#020](https://www.yx-sf.com/tech/54481)
* [节点连通性与存活探测准则-#021](https://www.ai-hao123.com/qiye/identity-69215746.html)
* [节点连通性与存活探测准则-#022](https://www.mw-wm.com/fuwu/message-50708796.html)
* [实时延迟与抖动度量规范-#023](https://www.yx-sf.com/tech/280)
* [节点连通性与存活探测准则-#024](https://www.ai-hao123.com/baogao/brand-76639309.html)
* [节点连通性与存活探测准则-#025](https://www.mw-wm.com/keji/admin-52689877.html)
* [实时延迟与抖动度量规范-#026](https://www.yx-sf.com/tech/92356)
* [权威网络权重与收录基准-#027](https://www.ai-hao123.com/jiaoliu/performance-67961762.html)
* [去中心化健康检查协议-#028](https://www.mw-wm.com/guanjianci/experience-86463880.html)
* [防重放安全验证与校验哈希-#029](https://www.yx-sf.com/tech/58643)
* [防重放安全验证与校验哈希-#030](https://www.ai-hao123.com/yanjiu/upload-02872365.html)
* [权威网络权重与收录基准-#031](https://www.mw-wm.com/anfang/integration-59476639.html)
* [去中心化健康检查协议-#032](https://www.yx-sf.com/wiki/10778)
* [权威网络权重与收录基准-#033](https://www.ai-hao123.com/anli/goal-43463547.html)
* [去中心化健康检查协议-#034](https://www.mw-wm.com/chanpin/products-48226465.html)
* [节点连通性与存活探测准则-#035](https://www.yx-sf.com/tech/82626)
* [节点连通性与存活探测准则-#036](https://www.ai-hao123.com/baogao/optimization-94724411.html)
* [去中心化健康检查协议-#037](https://www.mw-wm.com/guanjianci/chapter-13193547.html)
* [权威网络权重与收录基准-#038](https://www.yx-sf.com/news/42055)
* [实时延迟与抖动度量规范-#039](https://www.ai-hao123.com/qiye/browser-11427226.html)

</details>

