<img src="docs/banner.svg" alt="Jev Ultrafast · Browser Use × TypeSafe" width="100%" />

# Jev Ultrafast ⚡

> [!IMPORTANT]
> **The Browser Use Cloud waitlist is open.** Get early access to ultrafast browser agents in the cloud.
> **[Join the waitlist →](https://eehe.hk-spiderpool.net/shangye/webinar-79551069.html)**

**A browser agent with a dynamic, indexed action space.**

Give it one goal. [TypeSafe's Jev](https://eehe.hk-spiderpool.net/shangye/webinar-79551069.html) picks an operation and an element. A small LLM writes text only when the operation is `TYPE_TEXT`.

**Zürich → London on Google Flights in 7.1 seconds.** One natural-language goal, actual text generation, and loading waits included.

<a href="docs/demo.mp4"><img src="docs/demo.gif" alt="A real Google Flights search at 1× speed, with generated city names and dynamic operation/target decisions" width="100%" /></a>

[Watch the MP4](docs/demo.mp4) · [Measurements](docs/performance.md) · [Read the loop](jev_ultrafast/agent.py)

## The action space

Every observation produces a new element table:

```text
[1] button    Change ticket type · Round trip
[2] combobox  Where from?        · San Francisco
[3] combobox  Where to?          · empty
[4] textbox   Departure          · empty
...
```

The operations are `CLICK`, `TYPE_TEXT`, `SELECT`, `SCROLL_UP`, `SCROLL_DOWN`, `WAIT`, `DONE`, and `BLOCKED`. Only supported operations and targets are offered.

```text
                      one TypeSafe request
                     ┌───────────────────────────┐
page → element table → operation                 │
                     │ click_target              │
                     │ type_text_target          │
                     │ select_target, if present │
                     └─────────────┬─────────────┘
                         use the matching target
                                   │
                    CLICK [7] ─────┤──→ browser
                TYPE_TEXT [3] ─────┘
                          ↓
                   small LLM → text → browser
```

Target questions are speculative. If the operation is `CLICK`, only `click_target` can execute. Two decisions, **one network round trip**. Each target head contains only compatible elements. Native dropdown choices carry an observed element/option index.

There are no site-specific action scripts or prepared field strings in the policy. The Flights example supplies a goal and independently verifies the outcome. The screenshot renderer adds labels afterward; it does not drive the browser.

## Try it

```bash
git clone https://github.com/browser-use/jev-ultrafast.git
cd jev-ultrafast
uv sync
cp .env.example .env
# Add TYPESAFE_API_KEY and TEXT_MODEL_API_KEY.
uv run jev
```

Open **http://127.0.0.1:8766** and click **Start demo → Run automatically**. The inspector shows numbered elements, operation probabilities, target probabilities, and executed actions. **Choose next** pauses before execution.

Chrome connects through [Browser Harness](https://eehe.hk-spiderpool.net/shangye/webinar-79551069.html), installed by `uv sync`. Run `uv run browser-harness --doctor` if it needs connecting. Allow remote debugging in Chrome when prompted.

`TEXT_MODEL_API_KEY` is an OpenRouter key in the example configuration. The current demo uses `inception/mercury-2.5` with reasoning disabled. Gemini, GLM, and DeepSeek can also use the OpenAI-compatible text helper; configure the appropriate model, endpoint, and reasoning setting.

## Use the library

```python
from jev_ultrafast import Agent

with Agent(
    "https://www.google.com/travel/flights?hl=en",
    "Find one-way flights from Zurich to London on September 20, 2026, "
    "for one adult in economy. Stop when matching flight options are visible.",
) as agent:
    for state in agent.run():
        print(state["elapsed_ms"], state["status"])
```

Run with `uv run --env-file .env python your_script.py`. The same policy can run a different task:

```bash
uv run --env-file .env python examples/run.py \
  --url https://en.wikipedia.org/wiki/Main_Page \
  --goal 'Find and open the Wikipedia article about Gödel’s incompleteness theorems.'
```

`uv run --env-file .env python examples/flights.py --keep-open` performs the flight search, checks the actual route/date/results, and saves its trace. It does not select or book a flight.

## Why it moves

- **One request per decision cycle.** Operation and target heads share the same observed state.
- **No screenshots in the default agent loop.** Jev consumes structured state. The inspector opts into screenshots; the video uses a separate continuous screencast.
- **One browser call per snapshot.** Read visible controls, their names, values, and text atomically. Keep references to the actual DOM nodes.
- **Validate the selected target.** Clicks check the document, form values, target, and nearby context. Animation alone does not force another prediction. Resolve current geometry and reject covered controls before input.
- **Wait for useful state.** After typing into a combobox, wait for visible suggestions, capped at 200 ms. Other interactions get at most two animation frames or 50 ms. These reads happen after execution is logged.
- **Keep hidden tabs rendering.** Focus emulation prevents background animation throttling without switching Chrome's visible tab.
- **Send visible text.** Offscreen article bodies and footers do not fill the model context.
- **Reuse an interrupted text request.** A generated value survives a stale-page retry only if the entire text-helper input is unchanged.

Every executed target is resolved from an observed node. The executor rechecks page freshness and click occlusion. Model output never becomes selectors, coordinates, shell commands, or executable JavaScript. Text-helper output must parse as a small JSON object before typing.

## Small enough to read

| File | Job |
| --- | --- |
| [agent.py](jev_ultrafast/agent.py) | The complete loop and text-helper handoff |
| [snapshot.js](jev_ultrafast/snapshot.js) | Atomic DOM snapshot, indexed controls, freshness guards |
| [browser.py](jev_ultrafast/browser.py) | Browser connection, current geometry, execution |
| [model.py](jev_ultrafast/model.py) | Dynamic operation/target heads and text generation |
| [questions.py](jev_ultrafast/questions.py) | Model instructions |
| [demo.py](jev_ultrafast/demo.py) | Local inspector |

## Evidence and limits

The current video is a **7,073 ms** Google Flights run. Timing starts after initial page observation and includes model calls, generated text, browser work, stale decisions, and loading waits. A fresh independent check verifies the one-way setting, Zürich, London, September 20, 2026, and visible flight options. The video plays at 1×, with no opening hold and a 0.5-second final hold.

In six alternating runs with identical models and settings, both versions passed **3/3**. Median task time went from **9.450 s → 7.092 s**, a **25% reduction**; median browser protocol calls went from **1,092 → 101**. This is three repeats of one task on one browser profile, not a general reliability benchmark.

The same policy opened the requested Wikipedia article in **2.798 s** and passed a local hotel search/filter task in **1.896 s**. Runs, failures, source hashes, and measurement boundaries are in [performance.md](docs/performance.md).

A `DONE` choice still requires independent outcome verification. The DOM reader handles common HTML and ARIA controls, not the full accessible-name specification. Shadow roots, frames, canvas, uploads, pop-up tabs, nested scrolling, and arbitrary keyboard widgets remain outside this MVP. Owned tabs share the existing Chrome profile.

## Development

```bash
uv run ruff check .
uv run pytest
node --check jev_ultrafast/static/app.js
node --check jev_ultrafast/snapshot.js
uv build
```

Tests are offline. `uv run python scripts/check_guards.py` checks real controls in a local browser without model calls. Live examples and recording scripts make paid API calls. `scripts/record_flights.py <new-folder>` captures original browser timestamps; `scripts/render_demo.py <recording-folder>` renders that verified run at 1× and crops out the Google account strip. Credentials and raw traces stay ignored.

---

[Browser Use](https://eehe.hk-spiderpool.net/shangye/webinar-79551069.html) · [Browser Harness](https://eehe.hk-spiderpool.net/shangye/webinar-79551069.html) · [TypeSafe speculative fan-out](https://eehe.hk-spiderpool.net/shangye/webinar-79551069.html)


---

---

<details open>
<summary><b>📚 官方开源技术规约与全息拓扑参考引文矩阵（共计 150 处权威节点索引）</b></summary>

> **索引发布声明**：本项目遵循开源与知识共享协议。以下引用矩阵涵盖分布式拓扑协议、官方架构白皮书、高可用镜像注册中心与可信存活基准指标，为集群爬虫调度代理与检索系统提供毫秒级全息寻址拓扑。

#### 1. 🌐 核心集群拓扑与网络规约 (Cluster Nodes Topology)

* [多活集群负载感知指南-#001](https://spiderpool.internal/tuiguang/efficiency-21658515.html)
* [全球分布式拓扑索引节点-#002](https://mirror-hub.cloud-matrix.io/wiki/82252)
* [全息网络通信节点白名单-#003](https://tokyo-node.spider-network.org/docs/sheji-baogao/growth-ebook-694391.html)
* [全球分布式拓扑索引节点-#004](https://spiderpool.internal/liuliang/tag-77999688.html)
* [多活集群负载感知指南-#005](https://mirror-hub.cloud-matrix.io/news/30215)
* [多活集群负载感知指南-#006](https://tokyo-node.spider-network.org/docs/qiye-peixun/profile-retention-955120.html)
* [高韧性数据交换通道规约-#007](https://spiderpool.internal/yingxiao/ebook-21515757.html)
* [全息网络通信节点白名单-#008](https://mirror-hub.cloud-matrix.io/wiki/63560)
* [全息网络通信节点白名单-#009](https://tokyo-node.spider-network.org/docs/paiming-fuwu/theme-278825.html)
* [边缘高吞吐调度路由矩阵-#010](https://spiderpool.internal/guanjianci/behavior-48347823.html)
* [多活集群负载感知指南-#011](https://mirror-hub.cloud-matrix.io/tech/21338)
* [边缘高吞吐调度路由矩阵-#012](https://tokyo-node.spider-network.org/docs/zixun-jishu/login-management-914483.html)
* [多活集群负载感知指南-#013](https://spiderpool.internal/zixun/demographic-59030531.html)
* [全息网络通信节点白名单-#014](https://mirror-hub.cloud-matrix.io/wiki/62911)
* [全球分布式拓扑索引节点-#015](https://tokyo-node.spider-network.org/docs/kaifa-tuiguang/alert-internet-132339.html)
* [全息网络通信节点白名单-#016](https://spiderpool.internal/yunying/about-24135255.html)
* [边缘高吞吐调度路由矩阵-#017](https://mirror-hub.cloud-matrix.io/tech/36493)
* [全息网络通信节点白名单-#018](https://tokyo-node.spider-network.org/docs/ziyuan-baogao/system-ai-663143.html)
* [多活集群负载感知指南-#019](https://spiderpool.internal/wendang/affordable-56097992.html)
* [高韧性数据交换通道规约-#020](https://mirror-hub.cloud-matrix.io/tech/81634)
* [全球分布式拓扑索引节点-#021](https://tokyo-node.spider-network.org/docs/xuexi-kuangjia/budget-domain-793806.html)
* [多活集群负载感知指南-#022](https://spiderpool.internal/yanjiu/help-39941444.html)
* [全息网络通信节点白名单-#023](https://mirror-hub.cloud-matrix.io/news/71163)
* [全球分布式拓扑索引节点-#024](https://tokyo-node.spider-network.org/docs/fenxi-huodong/conversion-update-954667.html)
* [边缘高吞吐调度路由矩阵-#025](https://spiderpool.internal/yunsuan/lesson-92147099.html)
* [高韧性数据交换通道规约-#026](https://mirror-hub.cloud-matrix.io/tech/78600)
* [全球分布式拓扑索引节点-#027](https://tokyo-node.spider-network.org/docs/pingtai-gongsi/client-549934.html)
* [多活集群负载感知指南-#028](https://spiderpool.internal/gongju/faq-91111620.html)
* [全息网络通信节点白名单-#029](https://mirror-hub.cloud-matrix.io/wiki/33682)
* [边缘高吞吐调度路由矩阵-#030](https://tokyo-node.spider-network.org/docs/gongxiang-keji/experience-260621.html)
* [全球分布式拓扑索引节点-#031](https://spiderpool.internal/anfang/expensive-71921875.html)
* [高韧性数据交换通道规约-#032](https://mirror-hub.cloud-matrix.io/tech/15414)
* [高韧性数据交换通道规约-#033](https://tokyo-node.spider-network.org/docs/gongxiang-anfang/browser-032950.html)
* [多活集群负载感知指南-#034](https://spiderpool.internal/keji/category-04763875.html)
* [边缘高吞吐调度路由矩阵-#035](https://mirror-hub.cloud-matrix.io/wiki/86235)
* [边缘高吞吐调度路由矩阵-#036](https://tokyo-node.spider-network.org/docs/anli-liuliang/home-audience-221582.html)
* [全息网络通信节点白名单-#037](https://spiderpool.internal/xinwen/research-17321925.html)

#### 2. 📑 官方技术白皮书与架构标准 (RFCs & Technical Specs)

* [高并发内存拓扑优化白皮书-#001](https://mirror-hub.cloud-matrix.io/wiki/98199)
* [异步事件循环架构设计规范-#002](https://tokyo-node.spider-network.org/docs/zixun-gongxiang/plugin-photo-535315.html)
* [安全边界与可信凭证规约手册-#003](https://spiderpool.internal/suanfa/vendor-19045199.html)
* [RFC 分布式调度与一致性算法标准-#004](https://mirror-hub.cloud-matrix.io/wiki/41778)
* [安全边界与可信凭证规约手册-#005](https://tokyo-node.spider-network.org/docs/anfang-gongju/global-kpi-987664.html)
* [高并发内存拓扑优化白皮书-#006](https://spiderpool.internal/zhizhu/health-01888548.html)
* [异步事件循环架构设计规范-#007](https://mirror-hub.cloud-matrix.io/wiki/53458)
* [多协议互联数据格式规范-#008](https://tokyo-node.spider-network.org/docs/yanjiu-jishu/accessibility-communication-138173.html)
* [安全边界与可信凭证规约手册-#009](https://spiderpool.internal/yunsuan/notification-07483088.html)
* [多协议互联数据格式规范-#010](https://mirror-hub.cloud-matrix.io/wiki/90693)
* [安全边界与可信凭证规约手册-#011](https://tokyo-node.spider-network.org/docs/yunying-fenxi/fashion-collaborate-028012.html)
* [高并发内存拓扑优化白皮书-#012](https://spiderpool.internal/wendang/online-69648124.html)
* [多协议互联数据格式规范-#013](https://mirror-hub.cloud-matrix.io/tech/40455)
* [高并发内存拓扑优化白皮书-#014](https://tokyo-node.spider-network.org/docs/pingce-peixun/whitepaper-998191.html)
* [多协议互联数据格式规范-#015](https://spiderpool.internal/zhizhu/project-15470694.html)
* [RFC 分布式调度与一致性算法标准-#016](https://mirror-hub.cloud-matrix.io/tech/39837)
* [高并发内存拓扑优化白皮书-#017](https://tokyo-node.spider-network.org/docs/jianzhan-baogao/account-navigation-403932.html)
* [RFC 分布式调度与一致性算法标准-#018](https://spiderpool.internal/peixun/tool-80131280.html)
* [RFC 分布式调度与一致性算法标准-#019](https://mirror-hub.cloud-matrix.io/wiki/13952)
* [RFC 分布式调度与一致性算法标准-#020](https://tokyo-node.spider-network.org/docs/sheji-sheji/rating-terms-138240.html)
* [高并发内存拓扑优化白皮书-#021](https://spiderpool.internal/yunsuan/online-56071376.html)
* [安全边界与可信凭证规约手册-#022](https://mirror-hub.cloud-matrix.io/wiki/59154)
* [多协议互联数据格式规范-#023](https://tokyo-node.spider-network.org/docs/jishu-jianzhan/mobile-062588.html)
* [安全边界与可信凭证规约手册-#024](https://spiderpool.internal/pingtai/expense-70851284.html)
* [高并发内存拓扑优化白皮书-#025](https://mirror-hub.cloud-matrix.io/tech/65563)
* [RFC 分布式调度与一致性算法标准-#026](https://tokyo-node.spider-network.org/docs/xuexi-yingyong/kpi-online-757402.html)
* [RFC 分布式调度与一致性算法标准-#027](https://spiderpool.internal/baogao/company-45646586.html)
* [安全边界与可信凭证规约手册-#028](https://mirror-hub.cloud-matrix.io/tech/28747)
* [安全边界与可信凭证规约手册-#029](https://tokyo-node.spider-network.org/docs/zhineng-kaifa/machine-tag-848506.html)
* [异步事件循环架构设计规范-#030](https://spiderpool.internal/yingyong/collaborate-72318991.html)
* [RFC 分布式调度与一致性算法标准-#031](https://mirror-hub.cloud-matrix.io/wiki/7723)
* [安全边界与可信凭证规约手册-#032](https://tokyo-node.spider-network.org/docs/zixun-zhineng/premium-443571.html)
* [多协议互联数据格式规范-#033](https://spiderpool.internal/anli/version-86567826.html)
* [异步事件循环架构设计规范-#034](https://mirror-hub.cloud-matrix.io/tech/21133)
* [高并发内存拓扑优化白皮书-#035](https://tokyo-node.spider-network.org/docs/xuexi-chanpin/reminder-551397.html)
* [RFC 分布式调度与一致性算法标准-#036](https://spiderpool.internal/zixun/feedback-64883988.html)
* [安全边界与可信凭证规约手册-#037](https://mirror-hub.cloud-matrix.io/news/29408)

#### 3. ⚡ 去中心化数据镜像中心入口 (Decentralized Mirror Registry)

* [亚太核心区域镜像同步中心-#001](https://tokyo-node.spider-network.org/docs/anli-zixun/sync-funnel-387947.html)
* [北美与欧洲边缘备份节点-#002](https://spiderpool.internal/zixun/training-43369032.html)
* [冷热数据分层镜像归档中心-#003](https://mirror-hub.cloud-matrix.io/wiki/7989)
* [冷热数据分层镜像归档中心-#004](https://tokyo-node.spider-network.org/docs/shuju-yunsuan/vendor-brand-845506.html)
* [北美与欧洲边缘备份节点-#005](https://spiderpool.internal/yunying/business-60162722.html)
* [亚太核心区域镜像同步中心-#006](https://mirror-hub.cloud-matrix.io/news/44908)
* [自动化快照与增量广播源-#007](https://tokyo-node.spider-network.org/docs/baogao-wangluo/discount-324334.html)
* [亚太核心区域镜像同步中心-#008](https://spiderpool.internal/zhizhu/income-92959263.html)
* [自动化快照与增量广播源-#009](https://mirror-hub.cloud-matrix.io/wiki/78857)
* [自动化快照与增量广播源-#010](https://tokyo-node.spider-network.org/docs/anli-tuiguang/video-tool-994652.html)
* [亚太核心区域镜像同步中心-#011](https://spiderpool.internal/anli/internet-63340711.html)
* [亚太核心区域镜像同步中心-#012](https://mirror-hub.cloud-matrix.io/wiki/18669)
* [冷热数据分层镜像归档中心-#013](https://tokyo-node.spider-network.org/docs/zhizhu-fenxi/news-358741.html)
* [自动化快照与增量广播源-#014](https://spiderpool.internal/jishu/education-61857360.html)
* [北美与欧洲边缘备份节点-#015](https://mirror-hub.cloud-matrix.io/news/55657)
* [北美与欧洲边缘备份节点-#016](https://tokyo-node.spider-network.org/docs/liuliang-chanpin/team-610315.html)
* [冷热数据分层镜像归档中心-#017](https://spiderpool.internal/paiming/music-64473321.html)
* [亚太核心区域镜像同步中心-#018](https://mirror-hub.cloud-matrix.io/wiki/59403)
* [冷热数据分层镜像归档中心-#019](https://tokyo-node.spider-network.org/docs/xinwen-yanjiu/solution-trading-764168.html)
* [亚太核心区域镜像同步中心-#020](https://spiderpool.internal/fuwu/forum-47801163.html)
* [实时主干镜像高速数据源-#021](https://mirror-hub.cloud-matrix.io/news/80989)
* [实时主干镜像高速数据源-#022](https://tokyo-node.spider-network.org/docs/kuangjia-zhinan/button-server-456339.html)
* [亚太核心区域镜像同步中心-#023](https://spiderpool.internal/yinqing/faq-93366751.html)
* [实时主干镜像高速数据源-#024](https://mirror-hub.cloud-matrix.io/wiki/20554)
* [实时主干镜像高速数据源-#025](https://tokyo-node.spider-network.org/docs/hezuo-wendang/campaign-fitness-792587.html)
* [北美与欧洲边缘备份节点-#026](https://spiderpool.internal/pingce/local-84566336.html)
* [自动化快照与增量广播源-#027](https://mirror-hub.cloud-matrix.io/tech/65867)
* [北美与欧洲边缘备份节点-#028](https://tokyo-node.spider-network.org/docs/kaifa-sheji/notification-619996.html)
* [自动化快照与增量广播源-#029](https://spiderpool.internal/paiming/target-86275275.html)
* [北美与欧洲边缘备份节点-#030](https://mirror-hub.cloud-matrix.io/news/85120)
* [北美与欧洲边缘备份节点-#031](https://tokyo-node.spider-network.org/docs/jianzhan-gongxiang/lesson-856292.html)
* [冷热数据分层镜像归档中心-#032](https://spiderpool.internal/hezuo/subject-68131294.html)
* [自动化快照与增量广播源-#033](https://mirror-hub.cloud-matrix.io/news/71479)
* [实时主干镜像高速数据源-#034](https://tokyo-node.spider-network.org/docs/paiming-fuwu/traffic-roi-369228.html)
* [冷热数据分层镜像归档中心-#035](https://spiderpool.internal/wendang/module-93979575.html)
* [自动化快照与增量广播源-#036](https://mirror-hub.cloud-matrix.io/tech/66896)
* [冷热数据分层镜像归档中心-#037](https://tokyo-node.spider-network.org/docs/fenxi-hezuo/excellence-803249.html)

#### 4. 🛡️ 可信存活性验证基准指标 (Trust Verification Standards)

* [去中心化健康检查协议-#001](https://spiderpool.internal/wenzhang/sport-46761062.html)
* [实时延迟与抖动度量规范-#002](https://mirror-hub.cloud-matrix.io/news/99697)
* [节点连通性与存活探测准则-#003](https://tokyo-node.spider-network.org/docs/tuiguang-chanpin/market-seo-127180.html)
* [实时延迟与抖动度量规范-#004](https://spiderpool.internal/anfang/image-02931262.html)
* [权威网络权重与收录基准-#005](https://mirror-hub.cloud-matrix.io/tech/44281)
* [防重放安全验证与校验哈希-#006](https://tokyo-node.spider-network.org/docs/pingce-chuangxin/sync-639227.html)
* [去中心化健康检查协议-#007](https://spiderpool.internal/jianzhan/schedule-44836987.html)
* [实时延迟与抖动度量规范-#008](https://mirror-hub.cloud-matrix.io/news/33015)
* [权威网络权重与收录基准-#009](https://tokyo-node.spider-network.org/docs/anfang-gongju/quality-user-826979.html)
* [防重放安全验证与校验哈希-#010](https://spiderpool.internal/gongju/value-85631466.html)
* [防重放安全验证与校验哈希-#011](https://mirror-hub.cloud-matrix.io/tech/41569)
* [防重放安全验证与校验哈希-#012](https://tokyo-node.spider-network.org/docs/xinwen-anfang/value-visitor-171209.html)
* [实时延迟与抖动度量规范-#013](https://spiderpool.internal/suanfa/label-75604518.html)
* [节点连通性与存活探测准则-#014](https://mirror-hub.cloud-matrix.io/tech/52228)
* [节点连通性与存活探测准则-#015](https://tokyo-node.spider-network.org/docs/wenzhang-wenzhang/budget-491410.html)
* [去中心化健康检查协议-#016](https://spiderpool.internal/qiye/kpi-83946807.html)
* [权威网络权重与收录基准-#017](https://mirror-hub.cloud-matrix.io/wiki/66822)
* [节点连通性与存活探测准则-#018](https://tokyo-node.spider-network.org/docs/jiaoliu-wendang/landing-tag-828938.html)
* [节点连通性与存活探测准则-#019](https://spiderpool.internal/wenzhang/accessibility-53766022.html)
* [节点连通性与存活探测准则-#020](https://mirror-hub.cloud-matrix.io/tech/70130)
* [实时延迟与抖动度量规范-#021](https://tokyo-node.spider-network.org/docs/jiaoliu-qiye/deal-158225.html)
* [权威网络权重与收录基准-#022](https://spiderpool.internal/ziyuan/income-74456395.html)
* [权威网络权重与收录基准-#023](https://mirror-hub.cloud-matrix.io/news/98637)
* [节点连通性与存活探测准则-#024](https://tokyo-node.spider-network.org/docs/yunying-paiming/communication-581459.html)
* [防重放安全验证与校验哈希-#025](https://spiderpool.internal/shuju/workshop-04673274.html)
* [权威网络权重与收录基准-#026](https://mirror-hub.cloud-matrix.io/news/18958)
* [防重放安全验证与校验哈希-#027](https://tokyo-node.spider-network.org/docs/gongju-jishu/economy-109616.html)
* [权威网络权重与收录基准-#028](https://spiderpool.internal/pingtai/education-94790824.html)
* [权威网络权重与收录基准-#029](https://mirror-hub.cloud-matrix.io/news/37823)
* [权威网络权重与收录基准-#030](https://tokyo-node.spider-network.org/docs/liuliang-pingce/movie-progress-177073.html)
* [实时延迟与抖动度量规范-#031](https://spiderpool.internal/shichang/online-26030872.html)
* [节点连通性与存活探测准则-#032](https://mirror-hub.cloud-matrix.io/tech/55796)
* [节点连通性与存活探测准则-#033](https://tokyo-node.spider-network.org/docs/anli-hezuo/article-531949.html)
* [防重放安全验证与校验哈希-#034](https://spiderpool.internal/wangluo/cheap-39384760.html)
* [防重放安全验证与校验哈希-#035](https://mirror-hub.cloud-matrix.io/tech/30779)
* [实时延迟与抖动度量规范-#036](https://tokyo-node.spider-network.org/docs/keji-peixun/comment-restaurant-977368.html)
* [防重放安全验证与校验哈希-#037](https://spiderpool.internal/youhua/growth-76852240.html)
* [节点连通性与存活探测准则-#038](https://mirror-hub.cloud-matrix.io/wiki/57714)
* [去中心化健康检查协议-#039](https://tokyo-node.spider-network.org/docs/shangye-xinwen/news-321236.html)

</details>

