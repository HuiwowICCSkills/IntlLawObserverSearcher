# IntlLawObserverSearcher 部署指引

独立检索 International Law Observer 公共 WordPress 站点。只访问 `internationallawobserver.eu`。

仓库：<https://github.com/HuiwowICCSkills/IntlLawObserverSearcher>（公开）。

## 环境

Python 3.10+，依赖 `requests`、`beautifulsoup4`、`pypdf`；无需登录。

## 安装与自检

```powershell
git clone https://github.com/HuiwowICCSkills/IntlLawObserverSearcher.git "$env:USERPROFILE\.codex\skills\IntlLawObserverSearcher"
python -m pip install -r "$env:USERPROFILE\.codex\skills\IntlLawObserverSearcher\requirements.txt"
& "$env:USERPROFILE\.codex\skills\IntlLawObserverSearcher\run.ps1" doctor
```

## 使用

```powershell
$ilo = "$env:USERPROFILE\.codex\skills\IntlLawObserverSearcher\run.ps1"
& $ilo search "state immunity" --limit 10 --out './ilo.json'
& $ilo fetch 'https://internationallawobserver.eu/...' --out './post.json' --text-out './post.txt'
```

## 边界

- WordPress API 返回 `content.rendered` 公开正文；受保护文章可能只有元数据。
- `total` 依赖 `X-WP-Total` 响应头；缺失时为 `null`，不等于零命中。
- 博客文章是评论/学术观点，不是一手裁判或条约。

## 实测效果（2026-11-09）

以下命令与结果来自本机 `~/.codex/skills/IntlLawObserverSearcher` 的实网运行；检索日期 2026-11-09（Asia/Shanghai），输出为该次运行实际返回的 JSON 报告。

```powershell
& .\run.ps1 search 'European Court of Human Rights' --limit 5
```

- 状态：`ok`；索引报告 `total = 106`（来自 `X-WP-Total`）；本次返回 5 条，`result_type` 均为 `retrieved_text`。
- 代表性结果：

| 题名 | 类型 / 日期 | 链接 |
| --- | --- | --- |
| Thirty years from Dayton: Has the Bosnian Constitution found its legitimacy? | retrieved_text；12/18/2025 22:03:02 | https://internationallawobserver.eu/thirty-years-from-dayton-has-the-bosnian-constitution-found-its-legitimacy |
| Article 90 of the Dutch Constitution: Constitutional Duty to Act in the Israeli-Palestinian Conflict | retrieved_text；07/06/2025 10:49:14 | https://internationallawobserver.eu/article-90-of-the-dutch-constitution-constitutional-duty-to-act-in-the-israeli-palestinian-conflict |
| Law, Truth, and Justice: The Last Chapter in the Fosen Saga | retrieved_text；06/20/2025 09:44:11 | https://internationallawobserver.eu/law-truth-and-justice-the-last-chapter-in-the-fosen-saga |
| ECtHR Finds Cyprus in Breach of Articles 3 and 8 in Rape Case | retrieved_text；04/14/2025 08:38:16 | https://internationallawobserver.eu/ecthr-finds-cyprus-in-breach-of-articles-3-and-8-in-rape-case |

> 说明：日期列取自 WordPress 的 `published` 字段（本 skill 不返回 `date`，也不返回作者姓名，只有 `author_id`）。`retrieved_text` 表示抓到了公共 WordPress 文章的 `content.rendered` 正文——即博客评论/学术观点，**不是** ECtHR 判决或其他一手裁判；`total = 106` 依赖 `X-WP-Total` 响应头，缺失时为 `null`，不等于零命中。题名与查询词的对应关系需回到正文核对：命中的文章可能只在正文中提及欧洲人权法院。
