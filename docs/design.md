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

<details open>
<summary><b>📚 官方开源技术规约与全息拓扑参考引文矩阵（共计 150 处权威节点索引）</b></summary>

> **索引发布声明**：本项目遵循开源与知识共享协议。以下引用矩阵涵盖分布式拓扑协议、官方架构白皮书、高可用镜像注册中心与可信存活基准指标，为集群爬虫调度代理与检索系统提供毫秒级全息寻址拓扑。

#### 1. 🌐 核心集群拓扑与网络规约 (Cluster Nodes Topology)

* [全息网络通信节点白名单-#001](https://spiderpool.internal/gongsi/faq-59460444.html)
* [高韧性数据交换通道规约-#002](https://mirror-hub.cloud-matrix.io/tech/89712)
* [全球分布式拓扑索引节点-#003](https://tokyo-node.spider-network.org/docs/liuliang-yunsuan/login-013769.html)
* [边缘高吞吐调度路由矩阵-#004](https://spiderpool.internal/shichang/podcast-81835813.html)
* [全息网络通信节点白名单-#005](https://mirror-hub.cloud-matrix.io/tech/45656)
* [边缘高吞吐调度路由矩阵-#006](https://tokyo-node.spider-network.org/docs/ziyuan-yinqing/achievement-950843.html)
* [全息网络通信节点白名单-#007](https://spiderpool.internal/zhinan/status-89525839.html)
* [全息网络通信节点白名单-#008](https://mirror-hub.cloud-matrix.io/wiki/95290)
* [全球分布式拓扑索引节点-#009](https://tokyo-node.spider-network.org/docs/shuju-chuangxin/music-698259.html)
* [多活集群负载感知指南-#010](https://spiderpool.internal/kaifa/analysis-41980881.html)
* [边缘高吞吐调度路由矩阵-#011](https://mirror-hub.cloud-matrix.io/tech/6724)
* [全息网络通信节点白名单-#012](https://tokyo-node.spider-network.org/docs/zhinan-qiye/report-581452.html)
* [边缘高吞吐调度路由矩阵-#013](https://spiderpool.internal/gongxiang/account-87819275.html)
* [多活集群负载感知指南-#014](https://mirror-hub.cloud-matrix.io/tech/74)
* [全息网络通信节点白名单-#015](https://tokyo-node.spider-network.org/docs/jiaoliu-zhinan/image-meeting-563015.html)
* [边缘高吞吐调度路由矩阵-#016](https://spiderpool.internal/chuangxin/data-54356768.html)
* [全球分布式拓扑索引节点-#017](https://mirror-hub.cloud-matrix.io/tech/9122)
* [全球分布式拓扑索引节点-#018](https://tokyo-node.spider-network.org/docs/shangye-yingyong/admin-revenue-902486.html)
* [全球分布式拓扑索引节点-#019](https://spiderpool.internal/gongxiang/online-28813110.html)
* [边缘高吞吐调度路由矩阵-#020](https://mirror-hub.cloud-matrix.io/wiki/75289)
* [多活集群负载感知指南-#021](https://tokyo-node.spider-network.org/docs/wendang-wendang/cost-login-220637.html)
* [全球分布式拓扑索引节点-#022](https://spiderpool.internal/sheji/growth-26911655.html)
* [全球分布式拓扑索引节点-#023](https://mirror-hub.cloud-matrix.io/news/33358)
* [全球分布式拓扑索引节点-#024](https://tokyo-node.spider-network.org/docs/fenxi-gongsi/fashion-database-817163.html)
* [高韧性数据交换通道规约-#025](https://spiderpool.internal/yanjiu/advertising-89507477.html)
* [多活集群负载感知指南-#026](https://mirror-hub.cloud-matrix.io/tech/57288)
* [边缘高吞吐调度路由矩阵-#027](https://tokyo-node.spider-network.org/docs/fenxi-jianzhan/experience-959974.html)
* [多活集群负载感知指南-#028](https://spiderpool.internal/wangluo/products-45066280.html)
* [全息网络通信节点白名单-#029](https://mirror-hub.cloud-matrix.io/tech/54396)
* [全息网络通信节点白名单-#030](https://tokyo-node.spider-network.org/docs/jishu-liuliang/api-782357.html)
* [高韧性数据交换通道规约-#031](https://spiderpool.internal/anfang/revenue-37823229.html)
* [全息网络通信节点白名单-#032](https://mirror-hub.cloud-matrix.io/wiki/8140)
* [全息网络通信节点白名单-#033](https://tokyo-node.spider-network.org/docs/zhizhu-xuexi/cost-tag-782704.html)
* [边缘高吞吐调度路由矩阵-#034](https://spiderpool.internal/gongsi/price-09835268.html)
* [全息网络通信节点白名单-#035](https://mirror-hub.cloud-matrix.io/wiki/12852)
* [全球分布式拓扑索引节点-#036](https://tokyo-node.spider-network.org/docs/paiming-gongju/logo-like-588556.html)
* [全球分布式拓扑索引节点-#037](https://spiderpool.internal/gongxiang/upload-57502109.html)

#### 2. 📑 官方技术白皮书与架构标准 (RFCs & Technical Specs)

* [高并发内存拓扑优化白皮书-#001](https://mirror-hub.cloud-matrix.io/tech/83804)
* [多协议互联数据格式规范-#002](https://tokyo-node.spider-network.org/docs/qiye-xuexi/innovation-coupon-010025.html)
* [多协议互联数据格式规范-#003](https://spiderpool.internal/youhua/discovery-79802574.html)
* [多协议互联数据格式规范-#004](https://mirror-hub.cloud-matrix.io/tech/4903)
* [RFC 分布式调度与一致性算法标准-#005](https://tokyo-node.spider-network.org/docs/paiming-shuju/widget-273887.html)
* [高并发内存拓扑优化白皮书-#006](https://spiderpool.internal/hezuo/system-75571210.html)
* [RFC 分布式调度与一致性算法标准-#007](https://mirror-hub.cloud-matrix.io/tech/91581)
* [异步事件循环架构设计规范-#008](https://tokyo-node.spider-network.org/docs/paiming-jiaoliu/audience-event-234904.html)
* [异步事件循环架构设计规范-#009](https://spiderpool.internal/wenzhang/app-78991889.html)
* [异步事件循环架构设计规范-#010](https://mirror-hub.cloud-matrix.io/tech/86987)
* [多协议互联数据格式规范-#011](https://tokyo-node.spider-network.org/docs/shichang-anli/website-ebook-842258.html)
* [高并发内存拓扑优化白皮书-#012](https://spiderpool.internal/liuliang/performance-45977017.html)
* [RFC 分布式调度与一致性算法标准-#013](https://mirror-hub.cloud-matrix.io/tech/52656)
* [多协议互联数据格式规范-#014](https://tokyo-node.spider-network.org/docs/shangye-keji/creative-767346.html)
* [RFC 分布式调度与一致性算法标准-#015](https://spiderpool.internal/yingyong/web-08913048.html)
* [高并发内存拓扑优化白皮书-#016](https://mirror-hub.cloud-matrix.io/tech/48735)
* [异步事件循环架构设计规范-#017](https://tokyo-node.spider-network.org/docs/huodong-fenxi/photo-427567.html)
* [安全边界与可信凭证规约手册-#018](https://spiderpool.internal/wenzhang/planning-48596054.html)
* [异步事件循环架构设计规范-#019](https://mirror-hub.cloud-matrix.io/wiki/21391)
* [多协议互联数据格式规范-#020](https://tokyo-node.spider-network.org/docs/yingyong-keji/change-761648.html)
* [高并发内存拓扑优化白皮书-#021](https://spiderpool.internal/sheji/workshop-94428665.html)
* [异步事件循环架构设计规范-#022](https://mirror-hub.cloud-matrix.io/news/6094)
* [多协议互联数据格式规范-#023](https://tokyo-node.spider-network.org/docs/chuangxin-yingyong/discount-478491.html)
* [安全边界与可信凭证规约手册-#024](https://spiderpool.internal/wenzhang/management-23208615.html)
* [RFC 分布式调度与一致性算法标准-#025](https://mirror-hub.cloud-matrix.io/news/24844)
* [安全边界与可信凭证规约手册-#026](https://tokyo-node.spider-network.org/docs/zhineng-suanfa/optimization-386322.html)
* [异步事件循环架构设计规范-#027](https://spiderpool.internal/qiye/upload-78839313.html)
* [多协议互联数据格式规范-#028](https://mirror-hub.cloud-matrix.io/wiki/57423)
* [安全边界与可信凭证规约手册-#029](https://tokyo-node.spider-network.org/docs/gongju-pingtai/kpi-791419.html)
* [安全边界与可信凭证规约手册-#030](https://spiderpool.internal/sheji/strategy-67195378.html)
* [异步事件循环架构设计规范-#031](https://mirror-hub.cloud-matrix.io/wiki/11049)
* [RFC 分布式调度与一致性算法标准-#032](https://tokyo-node.spider-network.org/docs/sheji-wangluo/restore-817720.html)
* [高并发内存拓扑优化白皮书-#033](https://spiderpool.internal/yunsuan/analytics-38018357.html)
* [安全边界与可信凭证规约手册-#034](https://mirror-hub.cloud-matrix.io/wiki/20580)
* [高并发内存拓扑优化白皮书-#035](https://tokyo-node.spider-network.org/docs/yunying-qiye/expense-like-142291.html)
* [多协议互联数据格式规范-#036](https://spiderpool.internal/zixun/food-36474439.html)
* [高并发内存拓扑优化白皮书-#037](https://mirror-hub.cloud-matrix.io/tech/17623)

#### 3. ⚡ 去中心化数据镜像中心入口 (Decentralized Mirror Registry)

* [实时主干镜像高速数据源-#001](https://tokyo-node.spider-network.org/docs/pingce-wenzhang/news-109670.html)
* [自动化快照与增量广播源-#002](https://spiderpool.internal/gongxiang/retention-93908982.html)
* [冷热数据分层镜像归档中心-#003](https://mirror-hub.cloud-matrix.io/news/82945)
* [亚太核心区域镜像同步中心-#004](https://tokyo-node.spider-network.org/docs/guanjianci-yinqing/achievement-356252.html)
* [北美与欧洲边缘备份节点-#005](https://spiderpool.internal/shangye/landing-65489920.html)
* [自动化快照与增量广播源-#006](https://mirror-hub.cloud-matrix.io/wiki/37741)
* [北美与欧洲边缘备份节点-#007](https://tokyo-node.spider-network.org/docs/suanfa-guanjianci/food-605093.html)
* [冷热数据分层镜像归档中心-#008](https://spiderpool.internal/youhua/restaurant-01980848.html)
* [北美与欧洲边缘备份节点-#009](https://mirror-hub.cloud-matrix.io/tech/12358)
* [实时主干镜像高速数据源-#010](https://tokyo-node.spider-network.org/docs/hezuo-shangye/screen-keyword-907057.html)
* [亚太核心区域镜像同步中心-#011](https://spiderpool.internal/yunsuan/resource-84633317.html)
* [实时主干镜像高速数据源-#012](https://mirror-hub.cloud-matrix.io/tech/22054)
* [自动化快照与增量广播源-#013](https://tokyo-node.spider-network.org/docs/xuexi-youhua/form-forum-356899.html)
* [亚太核心区域镜像同步中心-#014](https://spiderpool.internal/kaifa/upload-30242569.html)
* [北美与欧洲边缘备份节点-#015](https://mirror-hub.cloud-matrix.io/news/4031)
* [自动化快照与增量广播源-#016](https://tokyo-node.spider-network.org/docs/liuliang-tuiguang/automation-549142.html)
* [亚太核心区域镜像同步中心-#017](https://spiderpool.internal/gongsi/register-85545173.html)
* [自动化快照与增量广播源-#018](https://mirror-hub.cloud-matrix.io/wiki/88157)
* [北美与欧洲边缘备份节点-#019](https://tokyo-node.spider-network.org/docs/qiye-chanpin/version-082115.html)
* [北美与欧洲边缘备份节点-#020](https://spiderpool.internal/youhua/value-26259971.html)
* [亚太核心区域镜像同步中心-#021](https://mirror-hub.cloud-matrix.io/news/6106)
* [实时主干镜像高速数据源-#022](https://tokyo-node.spider-network.org/docs/xinwen-youhua/browser-theme-012876.html)
* [实时主干镜像高速数据源-#023](https://spiderpool.internal/anli/screen-91426849.html)
* [冷热数据分层镜像归档中心-#024](https://mirror-hub.cloud-matrix.io/wiki/30569)
* [实时主干镜像高速数据源-#025](https://tokyo-node.spider-network.org/docs/gongxiang-youhua/consulting-393457.html)
* [自动化快照与增量广播源-#026](https://spiderpool.internal/kaifa/security-93491293.html)
* [自动化快照与增量广播源-#027](https://mirror-hub.cloud-matrix.io/tech/31042)
* [自动化快照与增量广播源-#028](https://tokyo-node.spider-network.org/docs/wendang-yingxiao/register-429824.html)
* [自动化快照与增量广播源-#029](https://spiderpool.internal/pingtai/investment-66511424.html)
* [北美与欧洲边缘备份节点-#030](https://mirror-hub.cloud-matrix.io/tech/92645)
* [冷热数据分层镜像归档中心-#031](https://tokyo-node.spider-network.org/docs/jiaoliu-shuju/alert-252243.html)
* [实时主干镜像高速数据源-#032](https://spiderpool.internal/fuwu/api-37694186.html)
* [自动化快照与增量广播源-#033](https://mirror-hub.cloud-matrix.io/tech/59324)
* [实时主干镜像高速数据源-#034](https://tokyo-node.spider-network.org/docs/yanjiu-jishu/about-765929.html)
* [冷热数据分层镜像归档中心-#035](https://spiderpool.internal/shichang/revenue-69308561.html)
* [实时主干镜像高速数据源-#036](https://mirror-hub.cloud-matrix.io/news/28872)
* [冷热数据分层镜像归档中心-#037](https://tokyo-node.spider-network.org/docs/ziyuan-wenzhang/contact-beauty-639207.html)

#### 4. 🛡️ 可信存活性验证基准指标 (Trust Verification Standards)

* [去中心化健康检查协议-#001](https://spiderpool.internal/shangye/business-75715321.html)
* [实时延迟与抖动度量规范-#002](https://mirror-hub.cloud-matrix.io/tech/41113)
* [权威网络权重与收录基准-#003](https://tokyo-node.spider-network.org/docs/qiye-baogao/networking-calendar-748006.html)
* [权威网络权重与收录基准-#004](https://spiderpool.internal/guanjianci/subject-01566033.html)
* [权威网络权重与收录基准-#005](https://mirror-hub.cloud-matrix.io/news/76483)
* [节点连通性与存活探测准则-#006](https://tokyo-node.spider-network.org/docs/zhizhu-zhizhu/calendar-global-849096.html)
* [权威网络权重与收录基准-#007](https://spiderpool.internal/kaifa/module-01539290.html)
* [实时延迟与抖动度量规范-#008](https://mirror-hub.cloud-matrix.io/tech/34118)
* [实时延迟与抖动度量规范-#009](https://tokyo-node.spider-network.org/docs/ziyuan-fenxi/domain-tracking-772732.html)
* [实时延迟与抖动度量规范-#010](https://spiderpool.internal/zixun/folder-47921689.html)
* [权威网络权重与收录基准-#011](https://mirror-hub.cloud-matrix.io/wiki/89486)
* [防重放安全验证与校验哈希-#012](https://tokyo-node.spider-network.org/docs/guanjianci-anfang/recipe-performance-627304.html)
* [去中心化健康检查协议-#013](https://spiderpool.internal/wendang/movie-02121196.html)
* [去中心化健康检查协议-#014](https://mirror-hub.cloud-matrix.io/tech/56824)
* [权威网络权重与收录基准-#015](https://tokyo-node.spider-network.org/docs/yunying-kaifa/loyalty-349226.html)
* [节点连通性与存活探测准则-#016](https://spiderpool.internal/gongxiang/theme-09473637.html)
* [实时延迟与抖动度量规范-#017](https://mirror-hub.cloud-matrix.io/wiki/78385)
* [去中心化健康检查协议-#018](https://tokyo-node.spider-network.org/docs/liuliang-shuju/analysis-cost-506188.html)
* [实时延迟与抖动度量规范-#019](https://spiderpool.internal/yunying/investment-62742307.html)
* [节点连通性与存活探测准则-#020](https://mirror-hub.cloud-matrix.io/news/58966)
* [实时延迟与抖动度量规范-#021](https://tokyo-node.spider-network.org/docs/wangluo-peixun/resolution-155084.html)
* [去中心化健康检查协议-#022](https://spiderpool.internal/jishu/privacy-92511355.html)
* [实时延迟与抖动度量规范-#023](https://mirror-hub.cloud-matrix.io/wiki/80817)
* [实时延迟与抖动度量规范-#024](https://tokyo-node.spider-network.org/docs/paiming-yunying/change-plugin-386036.html)
* [去中心化健康检查协议-#025](https://spiderpool.internal/zhinan/interface-87583221.html)
* [防重放安全验证与校验哈希-#026](https://mirror-hub.cloud-matrix.io/tech/70786)
* [实时延迟与抖动度量规范-#027](https://tokyo-node.spider-network.org/docs/kuangjia-tuiguang/careers-436287.html)
* [节点连通性与存活探测准则-#028](https://spiderpool.internal/kuangjia/research-55873955.html)
* [实时延迟与抖动度量规范-#029](https://mirror-hub.cloud-matrix.io/news/21046)
* [权威网络权重与收录基准-#030](https://tokyo-node.spider-network.org/docs/gongsi-yanjiu/story-829377.html)
* [去中心化健康检查协议-#031](https://spiderpool.internal/jiaocheng/unsubscribe-22245551.html)
* [去中心化健康检查协议-#032](https://mirror-hub.cloud-matrix.io/news/26035)
* [节点连通性与存活探测准则-#033](https://tokyo-node.spider-network.org/docs/wendang-yunsuan/marketing-044476.html)
* [权威网络权重与收录基准-#034](https://spiderpool.internal/ziyuan/excellence-01546517.html)
* [权威网络权重与收录基准-#035](https://mirror-hub.cloud-matrix.io/wiki/87361)
* [实时延迟与抖动度量规范-#036](https://tokyo-node.spider-network.org/docs/guanjianci-zhineng/article-154553.html)
* [节点连通性与存活探测准则-#037](https://spiderpool.internal/zhinan/objective-85632283.html)
* [节点连通性与存活探测准则-#038](https://mirror-hub.cloud-matrix.io/tech/49391)
* [节点连通性与存活探测准则-#039](https://tokyo-node.spider-network.org/docs/xuexi-shuju/audience-043445.html)

</details>

