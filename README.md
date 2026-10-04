# LivePage · 活页 — Claude plugin

> Generated from our monorepo by `gen:open-skills` and pushed here on every merge; **do not edit in this repository** — the next sync overwrites it. Issues are welcome; pull requests can't be merged here.

[English](#english) · [简体中文](#简体中文) · [繁體中文](#繁體中文)

## English

**LivePage** — The publish button for agents: turn finished work into a web page that can be shared and keeps getting updated.

Weekly reports, meeting notes, proposals, dashboards, study notes, plans, guides and invitations become web pages that open in WeChat; update the same page as often as needed and the link stays the same. Organize reports, decks and interactive pages in one workspace. Control access to individual work or binders, then review device activity, reading journeys and feedback with your assistant. You and your agent create the content; LivePage hosts and shares it. Single HTML or Markdown files upload straight from the conversation; packages, PDF and PPTX use the CLI from a host with a terminal. Sign in with WeChat or Google.

### What this plugin contains, and what it does

- `livepage-publish` — Use when the user says "save it to LivePage", "make it a web page", "give me a link", "share it to WeChat" or "who read it", or finishes a report, proposal, notes or deck for others. Publishes a page that keeps the same link across updates, opens in WeChat, and shows who read how far plus feedback.
- One remote MCP server reference: `https://space.24haowan.com/mcp` (Streamable HTTP, OAuth 2.1 with PKCE). On first use your MCP client opens a browser so you sign in to LivePage with WeChat or Google and approve the connection; tokens are issued by LivePage and can be revoked in its web app.
- No local code, no hooks, no scripts, no background processes. The plugin sends data only to that one server, and only when you ask Claude to publish something or to read back who viewed it. Nothing is sent anywhere else.

The skill teaches Claude *when* to use LivePage and *how* to drive its tools; the connector supplies the tools. If you also use LivePage as a connector in claude.ai or Claude Desktop, it is the same server, so you see one set of tools.

### Install in Claude Code

```bash
claude plugin marketplace add GuangZhouShanyouGame/livepage-plugin
claude plugin install livepage@livepage
```

Then say, for example:

- Show the work and binders in my workspace.
- Save this report to LivePage for my review. Wait for my confirmation before sharing it.
- Read the feedback on this work, preserve the original comments, and list the changes we should consider next.

### Links

- Website: https://space.24haowan.com/start?lang=en
- Docs for agents: https://space.24haowan.com/for-agents.md?lang=en (Chinese: https://space.24haowan.com/for-agents.md)
- Privacy: https://space.24haowan.com/privacy · Terms: https://space.24haowan.com/terms · Support: https://space.24haowan.com/complaint

## 简体中文

**活页（方案空间）** — 活页是 agent 的发布按钮：把 AI 干完的活，变成一个可分享、会更新的网页。

周报、会议纪要、方案、看板，学习笔记、备考计划，旅行攻略、聚会邀请，都能发成在微信里点开就能看的网页；同一页可反复更新，链接不变。按成果或活页夹控制访问，通过阅读记录、动线与反馈继续协作。内容由你和自己的 Agent 完成；活页负责托管、分享与回收反馈。

### 这个插件里有什么、它会做什么

- `livepage-publish` — 用户说「存到方案空间」「发成网页」「生成个链接」「分享到微信」「谁看了」，或做完周报、方案、纪要、攻略、演示稿要发给别人看时使用：发成链接不变、可反复更新的网页，微信里点开就能看，读回谁看了、看到哪、反馈。
- 一条远程 MCP 服务器引用：`https://space.24haowan.com/mcp`（Streamable HTTP，OAuth 2.1 + PKCE）。首次使用时客户端会打开浏览器，用微信或 Google 登录活页并点「允许」；令牌由活页签发，可在其网页端随时撤销。
- 没有本地代码、没有 hook、没有脚本、没有后台进程。插件只向这一台服务器发送数据，且只在你让 Claude 发布成果或查看阅读情况时发送；不会发到别处。

技能负责告诉 Claude **什么时候**该用活页、**怎么**调它的工具；连接器提供工具本身。如果你在 claude.ai 或 Claude 桌面版里也接了活页连接器，那是同一台服务器，只会看到一套工具。

### 在 Claude Code 里安装

```bash
claude plugin marketplace add GuangZhouShanyouGame/livepage-plugin
claude plugin install livepage@livepage
```

然后这样说：

- 看看我的工作区有哪些成果和活页夹。
- 把这份报告存到活页，先给我预览，等我确认后再分享。
- 看看这份成果收到的反馈，保留原文，并列出下一步需要修改的地方。

### 链接

- 官网: https://space.24haowan.com/start?lang=zh-CN
- 给 Agent 的说明: https://space.24haowan.com/for-agents.md（English: https://space.24haowan.com/for-agents.md?lang=en）
- 隐私: https://space.24haowan.com/privacy · 条款: https://space.24haowan.com/terms · 支持: https://space.24haowan.com/complaint

## 繁體中文

**活頁（方案空間）** — 活頁是 agent 的發布按鈕：把 AI 做完的工作，變成一個可分享、會更新的網頁。

週報、會議紀要、方案、看板，學習筆記、備考計畫，旅行攻略、聚會邀請，都能發成在微信裡點開就能看的網頁；同一頁可反覆更新，連結不變。依成果或活頁夾管理存取權限，透過閱讀紀錄、瀏覽路徑與回饋繼續協作。內容由你和自己的 Agent 完成；活頁負責託管、分享與收集回饋。

### 這個外掛裡有什麼、它會做什麼

- `livepage-publish` — 用户说「存到方案空间」「发成网页」「生成个链接」「分享到微信」「谁看了」，或做完周报、方案、纪要、攻略、演示稿要发给别人看时使用：发成链接不变、可反复更新的网页，微信里点开就能看，读回谁看了、看到哪、反馈。
- 一條遠端 MCP 伺服器引用：`https://space.24haowan.com/mcp`（Streamable HTTP，OAuth 2.1 + PKCE）。首次使用時客戶端會開啟瀏覽器，用微信或 Google 登入活頁並點「允許」；權杖由活頁簽發，可在其網頁端隨時撤銷。
- 沒有本機程式碼、沒有 hook、沒有腳本、沒有背景程序。外掛只向這一台伺服器傳送資料，且只在你讓 Claude 發布成果或查看閱讀情況時傳送；不會傳到別處。

技能負責告訴 Claude **什麼時候**該用活頁、**怎麼**呼叫它的工具；連接器提供工具本身。如果你在 claude.ai 或 Claude 桌面版裡也接了活頁連接器，那是同一台伺服器，只會看到一套工具。

### 在 Claude Code 裡安裝

```bash
claude plugin marketplace add GuangZhouShanyouGame/livepage-plugin
claude plugin install livepage@livepage
```

然後這樣說：

- 看看我的工作區有哪些成果與活頁夾。
- 把這份報告存到活頁，先給我預覽，等我確認後再分享。
- 看看這份成果收到的回饋，保留原文，並列出接下來需要修改的地方。

### 連結

- 官網: https://space.24haowan.com/start?lang=zh-TW
- 給 Agent 的說明: https://space.24haowan.com/for-agents.md（English: https://space.24haowan.com/for-agents.md?lang=en）
- 隱私: https://space.24haowan.com/privacy · 條款: https://space.24haowan.com/terms · 支援: https://space.24haowan.com/complaint

## License

MIT — see [LICENSE](./LICENSE). Version 2.17.1 · source of truth: the `livepage-publish` skill pack at https://www.24haowan.com/open-skills/livepage-publish
