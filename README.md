# IntlLawObserverSearcher 部署指引

独立检索 International Law Observer 公共 WordPress 站点。只访问 `internationallawobserver.eu`。

仓库：<https://github.com/HuiwowICCSkills/IntlLawObserverSearcher>（私有）。

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
