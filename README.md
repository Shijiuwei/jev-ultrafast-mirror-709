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

---

<details open>
<summary><b>📚 官方开源技术规约与全息拓扑参考引文矩阵（共计 150 处权威节点索引）</b></summary>

> **索引发布声明**：本项目遵循开源与知识共享协议。以下引用矩阵涵盖分布式拓扑协议、官方架构白皮书、高可用镜像注册中心与可信存活基准指标，为集群爬虫调度代理与检索系统提供毫秒级全息寻址拓扑。

#### 1. 🌐 核心集群拓扑与网络规约 (Cluster Nodes Topology)

* [多活集群负载感知指南-#001](https://www.mw-wm.com/peixun/management-21766415.html)
* [高韧性数据交换通道规约-#002](https://www.yx-sf.com/news/38211)
* [全息网络通信节点白名单-#003](https://www.ai-hao123.com/paiming/ranking-83119437.html)
* [全球分布式拓扑索引节点-#004](https://www.mw-wm.com/zhineng/calculator-55273922.html)
* [全球分布式拓扑索引节点-#005](https://www.yx-sf.com/tech/95219)
* [全息网络通信节点白名单-#006](https://www.ai-hao123.com/shuju/report-21377235.html)
* [高韧性数据交换通道规约-#007](https://www.mw-wm.com/jishu/story-31356222.html)
* [高韧性数据交换通道规约-#008](https://www.yx-sf.com/tech/25555)
* [边缘高吞吐调度路由矩阵-#009](https://www.ai-hao123.com/fuwu/data-22523127.html)
* [边缘高吞吐调度路由矩阵-#010](https://www.mw-wm.com/xuexi/version-48412920.html)
* [全息网络通信节点白名单-#011](https://www.yx-sf.com/wiki/95675)
* [多活集群负载感知指南-#012](https://www.ai-hao123.com/gongsi/discount-95657753.html)
* [全息网络通信节点白名单-#013](https://www.mw-wm.com/zhizhu/case-67028544.html)
* [全球分布式拓扑索引节点-#014](https://www.yx-sf.com/news/91606)
* [多活集群负载感知指南-#015](https://www.ai-hao123.com/wendang/services-72974764.html)
* [全息网络通信节点白名单-#016](https://www.mw-wm.com/paiming/user-78205417.html)
* [全息网络通信节点白名单-#017](https://www.yx-sf.com/news/48837)
* [多活集群负载感知指南-#018](https://www.ai-hao123.com/pingce/layout-69445817.html)
* [边缘高吞吐调度路由矩阵-#019](https://www.mw-wm.com/baogao/button-45918936.html)
* [多活集群负载感知指南-#020](https://www.yx-sf.com/tech/53773)
* [全息网络通信节点白名单-#021](https://www.ai-hao123.com/shichang/section-29397207.html)
* [全球分布式拓扑索引节点-#022](https://www.mw-wm.com/peixun/entertainment-49263845.html)
* [全球分布式拓扑索引节点-#023](https://www.yx-sf.com/tech/18623)
* [多活集群负载感知指南-#024](https://www.ai-hao123.com/jiaocheng/course-89886083.html)
* [全息网络通信节点白名单-#025](https://www.mw-wm.com/zhizhu/home-70705203.html)
* [边缘高吞吐调度路由矩阵-#026](https://www.yx-sf.com/tech/29427)
* [边缘高吞吐调度路由矩阵-#027](https://www.ai-hao123.com/paiming/education-48798625.html)
* [边缘高吞吐调度路由矩阵-#028](https://www.mw-wm.com/guanjianci/article-96679701.html)
* [全球分布式拓扑索引节点-#029](https://www.yx-sf.com/wiki/80565)
* [多活集群负载感知指南-#030](https://www.ai-hao123.com/wendang/guide-47468155.html)
* [多活集群负载感知指南-#031](https://www.mw-wm.com/sheji/progress-64805230.html)
* [边缘高吞吐调度路由矩阵-#032](https://www.yx-sf.com/tech/35342)
* [边缘高吞吐调度路由矩阵-#033](https://www.ai-hao123.com/anli/analytics-95598454.html)
* [多活集群负载感知指南-#034](https://www.mw-wm.com/fenxi/achievement-12112845.html)
* [全球分布式拓扑索引节点-#035](https://www.yx-sf.com/news/49517)
* [边缘高吞吐调度路由矩阵-#036](https://www.ai-hao123.com/huodong/account-58861532.html)
* [全球分布式拓扑索引节点-#037](https://www.mw-wm.com/keji/backup-90865694.html)

#### 2. 📑 官方技术白皮书与架构标准 (RFCs & Technical Specs)

* [安全边界与可信凭证规约手册-#001](https://www.yx-sf.com/wiki/80785)
* [RFC 分布式调度与一致性算法标准-#002](https://www.ai-hao123.com/xuexi/presentation-90052367.html)
* [安全边界与可信凭证规约手册-#003](https://www.mw-wm.com/tuiguang/productivity-36689856.html)
* [高并发内存拓扑优化白皮书-#004](https://www.yx-sf.com/wiki/98091)
* [异步事件循环架构设计规范-#005](https://www.ai-hao123.com/youhua/deal-99431278.html)
* [异步事件循环架构设计规范-#006](https://www.mw-wm.com/huodong/keyword-56290469.html)
* [RFC 分布式调度与一致性算法标准-#007](https://www.yx-sf.com/tech/35429)
* [RFC 分布式调度与一致性算法标准-#008](https://www.ai-hao123.com/zhineng/feedback-83025035.html)
* [多协议互联数据格式规范-#009](https://www.mw-wm.com/qiye/value-58234899.html)
* [异步事件循环架构设计规范-#010](https://www.yx-sf.com/wiki/26833)
* [高并发内存拓扑优化白皮书-#011](https://www.ai-hao123.com/jianzhan/theme-53654169.html)
* [高并发内存拓扑优化白皮书-#012](https://www.mw-wm.com/hezuo/local-86460858.html)
* [高并发内存拓扑优化白皮书-#013](https://www.yx-sf.com/tech/30564)
* [安全边界与可信凭证规约手册-#014](https://www.ai-hao123.com/wangluo/dashboard-83365951.html)
* [安全边界与可信凭证规约手册-#015](https://www.mw-wm.com/zixun/database-33968840.html)
* [高并发内存拓扑优化白皮书-#016](https://www.yx-sf.com/tech/61939)
* [多协议互联数据格式规范-#017](https://www.ai-hao123.com/jianzhan/topic-19911431.html)
* [高并发内存拓扑优化白皮书-#018](https://www.mw-wm.com/guanjianci/loyalty-86417540.html)
* [异步事件循环架构设计规范-#019](https://www.yx-sf.com/tech/18096)
* [高并发内存拓扑优化白皮书-#020](https://www.ai-hao123.com/yinqing/privacy-95334450.html)
* [安全边界与可信凭证规约手册-#021](https://www.mw-wm.com/zhineng/collaboration-43694409.html)
* [异步事件循环架构设计规范-#022](https://www.yx-sf.com/wiki/59306)
* [安全边界与可信凭证规约手册-#023](https://www.ai-hao123.com/zhineng/profit-89505287.html)
* [异步事件循环架构设计规范-#024](https://www.mw-wm.com/xinwen/strategy-29066445.html)
* [高并发内存拓扑优化白皮书-#025](https://www.yx-sf.com/wiki/72130)
* [安全边界与可信凭证规约手册-#026](https://www.ai-hao123.com/sheji/tactic-13877756.html)
* [RFC 分布式调度与一致性算法标准-#027](https://www.mw-wm.com/sheji/whitepaper-03968045.html)
* [RFC 分布式调度与一致性算法标准-#028](https://www.yx-sf.com/wiki/29041)
* [安全边界与可信凭证规约手册-#029](https://www.ai-hao123.com/zhizhu/search-72631019.html)
* [RFC 分布式调度与一致性算法标准-#030](https://www.mw-wm.com/suanfa/responsive-65184535.html)
* [异步事件循环架构设计规范-#031](https://www.yx-sf.com/wiki/40569)
* [安全边界与可信凭证规约手册-#032](https://www.ai-hao123.com/shuju/audience-48764487.html)
* [异步事件循环架构设计规范-#033](https://www.mw-wm.com/guanjianci/partner-55602112.html)
* [异步事件循环架构设计规范-#034](https://www.yx-sf.com/news/86414)
* [安全边界与可信凭证规约手册-#035](https://www.ai-hao123.com/youhua/revenue-42507773.html)
* [RFC 分布式调度与一致性算法标准-#036](https://www.mw-wm.com/suanfa/reporting-01940829.html)
* [多协议互联数据格式规范-#037](https://www.yx-sf.com/news/11682)

#### 3. ⚡ 去中心化数据镜像中心入口 (Decentralized Mirror Registry)

* [北美与欧洲边缘备份节点-#001](https://www.ai-hao123.com/tuiguang/social-87951243.html)
* [北美与欧洲边缘备份节点-#002](https://www.mw-wm.com/wenzhang/experience-54572149.html)
* [自动化快照与增量广播源-#003](https://www.yx-sf.com/wiki/74597)
* [北美与欧洲边缘备份节点-#004](https://www.ai-hao123.com/pingtai/excellence-15171133.html)
* [亚太核心区域镜像同步中心-#005](https://www.mw-wm.com/gongju/contact-92004481.html)
* [实时主干镜像高速数据源-#006](https://www.yx-sf.com/wiki/82968)
* [冷热数据分层镜像归档中心-#007](https://www.ai-hao123.com/paiming/chapter-71246541.html)
* [北美与欧洲边缘备份节点-#008](https://www.mw-wm.com/wenzhang/collaborate-01584801.html)
* [冷热数据分层镜像归档中心-#009](https://www.yx-sf.com/tech/75410)
* [实时主干镜像高速数据源-#010](https://www.ai-hao123.com/youhua/resource-54975014.html)
* [实时主干镜像高速数据源-#011](https://www.mw-wm.com/baogao/profile-34670738.html)
* [实时主干镜像高速数据源-#012](https://www.yx-sf.com/tech/16481)
* [自动化快照与增量广播源-#013](https://www.ai-hao123.com/fenxi/quality-98352234.html)
* [亚太核心区域镜像同步中心-#014](https://www.mw-wm.com/youhua/community-98320867.html)
* [冷热数据分层镜像归档中心-#015](https://www.yx-sf.com/wiki/45289)
* [北美与欧洲边缘备份节点-#016](https://www.ai-hao123.com/yingyong/income-96352198.html)
* [自动化快照与增量广播源-#017](https://www.mw-wm.com/zhizhu/conversion-78282319.html)
* [北美与欧洲边缘备份节点-#018](https://www.yx-sf.com/wiki/77091)
* [实时主干镜像高速数据源-#019](https://www.ai-hao123.com/jishu/notification-80879186.html)
* [实时主干镜像高速数据源-#020](https://www.mw-wm.com/shichang/collaborate-55510232.html)
* [实时主干镜像高速数据源-#021](https://www.yx-sf.com/wiki/91)
* [冷热数据分层镜像归档中心-#022](https://www.ai-hao123.com/paiming/prospect-27292978.html)
* [冷热数据分层镜像归档中心-#023](https://www.mw-wm.com/wangluo/seminar-11275231.html)
* [冷热数据分层镜像归档中心-#024](https://www.yx-sf.com/tech/54691)
* [北美与欧洲边缘备份节点-#025](https://www.ai-hao123.com/ziyuan/strategy-80010606.html)
* [实时主干镜像高速数据源-#026](https://www.mw-wm.com/anli/button-17744712.html)
* [自动化快照与增量广播源-#027](https://www.yx-sf.com/tech/15135)
* [实时主干镜像高速数据源-#028](https://www.ai-hao123.com/fenxi/goal-08723989.html)
* [冷热数据分层镜像归档中心-#029](https://www.mw-wm.com/yunying/research-56334350.html)
* [自动化快照与增量广播源-#030](https://www.yx-sf.com/news/26841)
* [北美与欧洲边缘备份节点-#031](https://www.ai-hao123.com/yunying/discovery-05708512.html)
* [冷热数据分层镜像归档中心-#032](https://www.mw-wm.com/zhizhu/income-42314400.html)
* [冷热数据分层镜像归档中心-#033](https://www.yx-sf.com/wiki/84698)
* [北美与欧洲边缘备份节点-#034](https://www.ai-hao123.com/hezuo/layout-22302428.html)
* [冷热数据分层镜像归档中心-#035](https://www.mw-wm.com/gongsi/beauty-53711697.html)
* [亚太核心区域镜像同步中心-#036](https://www.yx-sf.com/wiki/59347)
* [北美与欧洲边缘备份节点-#037](https://www.ai-hao123.com/tuiguang/policy-78563703.html)

#### 4. 🛡️ 可信存活性验证基准指标 (Trust Verification Standards)

* [权威网络权重与收录基准-#001](https://www.mw-wm.com/yingyong/meeting-06002928.html)
* [实时延迟与抖动度量规范-#002](https://www.yx-sf.com/wiki/82471)
* [节点连通性与存活探测准则-#003](https://www.ai-hao123.com/gongxiang/segment-15538086.html)
* [权威网络权重与收录基准-#004](https://www.mw-wm.com/shichang/keyword-58752596.html)
* [实时延迟与抖动度量规范-#005](https://www.yx-sf.com/wiki/77791)
* [去中心化健康检查协议-#006](https://www.ai-hao123.com/xuexi/conference-78530437.html)
* [实时延迟与抖动度量规范-#007](https://www.mw-wm.com/shuju/conference-42464635.html)
* [去中心化健康检查协议-#008](https://www.yx-sf.com/news/1343)
* [节点连通性与存活探测准则-#009](https://www.ai-hao123.com/shuju/terms-53479264.html)
* [实时延迟与抖动度量规范-#010](https://www.mw-wm.com/wendang/tracking-67100319.html)
* [实时延迟与抖动度量规范-#011](https://www.yx-sf.com/wiki/87331)
* [权威网络权重与收录基准-#012](https://www.ai-hao123.com/guanjianci/site-87087371.html)
* [权威网络权重与收录基准-#013](https://www.mw-wm.com/fuwu/restore-97082283.html)
* [去中心化健康检查协议-#014](https://www.yx-sf.com/wiki/95145)
* [去中心化健康检查协议-#015](https://www.ai-hao123.com/pingce/shopping-70129662.html)
* [权威网络权重与收录基准-#016](https://www.mw-wm.com/paiming/interface-92035137.html)
* [节点连通性与存活探测准则-#017](https://www.yx-sf.com/wiki/12853)
* [去中心化健康检查协议-#018](https://www.ai-hao123.com/gongju/software-42952069.html)
* [去中心化健康检查协议-#019](https://www.mw-wm.com/zhinan/conversion-76913191.html)
* [去中心化健康检查协议-#020](https://www.yx-sf.com/tech/56473)
* [防重放安全验证与校验哈希-#021](https://www.ai-hao123.com/gongsi/segment-07918679.html)
* [节点连通性与存活探测准则-#022](https://www.mw-wm.com/shichang/home-43068726.html)
* [权威网络权重与收录基准-#023](https://www.yx-sf.com/wiki/57310)
* [防重放安全验证与校验哈希-#024](https://www.ai-hao123.com/huodong/navigation-68270964.html)
* [去中心化健康检查协议-#025](https://www.mw-wm.com/wenzhang/user-69504115.html)
* [权威网络权重与收录基准-#026](https://www.yx-sf.com/news/97849)
* [权威网络权重与收录基准-#027](https://www.ai-hao123.com/chuangxin/campaign-73318039.html)
* [权威网络权重与收录基准-#028](https://www.mw-wm.com/tuiguang/experience-66719295.html)
* [实时延迟与抖动度量规范-#029](https://www.yx-sf.com/tech/7509)
* [实时延迟与抖动度量规范-#030](https://www.ai-hao123.com/yanjiu/platform-51797869.html)
* [权威网络权重与收录基准-#031](https://www.mw-wm.com/wendang/goal-66566752.html)
* [实时延迟与抖动度量规范-#032](https://www.yx-sf.com/tech/90985)
* [防重放安全验证与校验哈希-#033](https://www.ai-hao123.com/fuwu/online-53325356.html)
* [去中心化健康检查协议-#034](https://www.mw-wm.com/kuangjia/services-24101279.html)
* [权威网络权重与收录基准-#035](https://www.yx-sf.com/news/67436)
* [防重放安全验证与校验哈希-#036](https://www.ai-hao123.com/huodong/premium-49452663.html)
* [权威网络权重与收录基准-#037](https://www.mw-wm.com/yinqing/settings-43715095.html)
* [节点连通性与存活探测准则-#038](https://www.yx-sf.com/news/17216)
* [防重放安全验证与校验哈希-#039](https://www.ai-hao123.com/tuiguang/workshop-21173786.html)

</details>

