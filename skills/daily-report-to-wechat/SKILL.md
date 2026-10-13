---
name: daily-report-to-wechat
description: "Use when the user says \"send today's report to WeChat\", \"daily report\", \"weekly report\", \"status update\" or \"progress update for my boss / team\", or wants a recurring report the same people open in WeChat. Publishes to LivePage: one link per report series, a new version each day, and read receipts showing who opened it and how far they read. 用户说「把今天的日报发到微信」「日报」「周报」「工作汇报」「进度同步给老板 / 团队」，或要一份每天都更新、同一群人在微信里看的汇报时使用：同一条链接、每天发新版本，读回谁看了、看到哪。 用戶說「把今天的日報發到微信」「日報」「週報」「工作匯報」「進度同步給主管 / 團隊」，或要一份每天都更新、同一群人在微信裡看的匯報時使用：同一條連結、每天發新版本，讀回誰看了、看到哪。"
---
# 日报 / 周报发到微信（活页）

这是「方案空间（活页）」`publish-to-wechat` 技能的**场景薄包装**：同一台 MCP 服务器（`https://space.24haowan.com/mcp`）、同一套工具，只是触发词与步骤按「每天 / 每周给同一群人发汇报」这个场景写。接入方式、托管契约、纪律与判据都在 `publish-to-wechat`，这里不重复；两份一起装时，日报 / 周报类请求先按这一份走。

**什么时候用**：用户说「把今天的日报发到微信」「日报」「周报」「工作汇报」「进度同步给老板 / 团队」，或者已经有一份汇报文稿（Markdown / HTML / PDF）要给固定的几个人在微信里看。

**场景的三个要点**

1. **一条链接，天天更新。** 一个汇报系列 = 一份成果（proposal）；每天的日报是它的**新版本**，链接不变，收件人昨天收藏的链接今天打开就是新的。不要每天新建一份 —— 那会让对方手里积一堆链接。
2. **发给固定的人。** 汇报通常只给老板 / 团队：可见性默认 `private`（只有工作区成员能看），或 `passcode`（给口令的人能看）；**不要**替用户选 `public`。
3. **知道谁看了。** 发完第二天用户最常问的是「老板看了没」—— 用 `get_engagement_summary` 读事实（谁的设备、看到哪、停留多久），只陈述设备行为，不揣测意向。

## 步骤

1. **找到或新建系列**：`list_proposals` 看有没有「X 日报 / 周报」这份；有就沿用它的 `proposal_id`，没有再建（第一次发时 `publish_file` 会自动创建）。
2. **确认文稿是终稿**：汇报正文由用户（或你）写好并经用户确认；你负责的是发出去那段活，不替用户编内容。
3. **发新版本**：单个 Markdown / HTML 文件直接 `publish_file`（已有系列就带 `proposal_id`，成为它的新版本）；PDF / PPTX 或多文件的包用 `create_cli_token` + `space-cli push`（凭据只放进程环境，不写进任何文件，用完即弃）。
4. **等版本就绪**：`get_version` 到 `preview_ready` / 已发布；`converting` 就稍后再查；`review` 表示平台在复核，别人还看不到。
5. **预览给用户看**：`preview_version`（或 `read_version` 自己核对正文）→ 用户点头后 `publish_version`。
6. **定可见性**：第一次发时 `set_visibility`（`private` / `passcode`），之后的版本沿用，不用每天重设。
7. **把链接交出去**：把默认链接给用户转发；用户在电脑上时提醒页面右下角「扫码到微信」。要推到微信：`push_to_wechat` **默认只推给用户本人**（工作区里有别的成员也一样，别人收不到），适合每天的定时日报；要推给工作区里所有人再传 `to: "members"`（**不发给客户**）。用户本人没绑微信 / 没关注服务号时回执会说清原因并附绑定码。
8. **第二天读回**：`get_engagement_summary`（看谁看了、看到哪）、`list_feedback`（页面上的评论）。有反馈就改稿、再按第 3 步发新版本。

## 让它每天自己更新（报告任务）

用户说「每天早上帮我整理一份」「让它每天自己更新」「别让我每天手动发」时，不用每天手动发新版本 —— 用 `report_task` 一把工具，三句话走完「建好 → 试跑 → 开启」：

1. **建好并试跑**：`report_task`（`action: "create"`，`request` 写用户要看什么、多少条、什么不要；`schedule: { every: "day", at: "08:00" }`；已有系列就带 `proposal_id`，没有就给 `title` 新建一页；`trial: true` 顺便试跑一次）。谁能看缺省 `private`；用户说要给别人看，先问一句再改。
2. **等试跑**：一般 5–15 分钟，好了会在用户的微信里提醒，试跑结果只有用户看得到，不发给任何人。
3. **开启**：用户看过、说「开启」⇒ `action: "enable"`（开启前必须先试跑成功过一次）；缺省把那份试跑发成这一页的第一版。之后到点就发成新版本，链接不变，照设置提醒用户、成员和关注这一页的读者。
4. **之后都是一句话**：改时间 / 补一句要求（request_append）/ 改谁能看 / 改提醒谁用 `action: "update"`，只传要改的；休假用 `action: "pause"`（`until` 那天自动恢复），`action: "resume"` 恢复，`action: "delete"` 不要了（这一页和已发的版本都还在），`action: "list"` 看有哪些。
5. **照着 `say` 说**：每个动作都返回一段可以原样转述的话，照它说，不要自己编事实；时间按用户的当地钟点说（07:30），不说别的格式。

## 不要做

- 不要每天新建一份成果 —— 一个系列一个 `proposal_id`。
- 不要替用户把汇报设成 `public`；不要把链接发给用户没点名的人。
- 不要自己拼链接、不要把文件直接贴给收件人 —— 链接只从工具返回里拿。
- 不要在用户没问的时候揣测「老板看了但没回」的含义；读到的是设备行为，照实说。

## English summary

Thin wrapper of `publish-to-wechat` for recurring reports: one LivePage proposal per report series, each day's report is a **new version** (same link), visibility `private` or `passcode` (never `public` on the user's behalf), and read receipts via `get_engagement_summary` the next day. Steps: `list_proposals` → confirm the text is final → `publish_file` (with `proposal_id` for an existing series; PDF/PPTX/multi-file via `create_cli_token` + `space-cli push`) → `get_version` until ready → `preview_version` and user approval → `publish_version` → `set_visibility` once → hand over the link (and `push_to_wechat`, which by default sends to the user only — pass `to: "members"` for everyone in the workspace) → `get_engagement_summary` / `list_feedback` tomorrow. To have the page updated on a schedule instead of publishing by hand, use `report_task`: `create` (with `trial: true`) → the user checks the trial (WeChat reminder, visible only to them) → `enable`; later `update` / `pause` / `resume` / `delete` / `list`, relaying each returned `say` as-is.
