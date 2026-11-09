# International Law Observer 实测记录（2026-11-09）

`https://internationallawobserver.eu/wp-json/wp/v2/posts`

参数：`search`、`per_page`、`offset`。

实测 `state immunity --limit 1` → `status=ok`、`total=23`、1 条；`content.rendered` 返回公开正文。`fetch` 文章页 → `text_characters=2157`。

边界：`X-WP-Total` 缺失时 `total=null`；受保护文章可能只有元数据。
