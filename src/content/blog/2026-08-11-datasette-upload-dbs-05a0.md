---
title: "datasette-upload-dbs 0.5a0"
description: "datasette-upload-dbs 0.5a0"
pubDate: "2026-08-11"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Aug/11/datasette-upload-dbs"
---

发布：datasette-upload-dbs 0.5a0 这个插件已经存在一段时间了——它允许用户将全新的 SQLite 数据库上传到托管的 Datasette 实例中，此后该数据库便会由该实例提供服务。它还可以用于将数据库原子性地替换为更新的版本。上传的数据库会被保存到文件，经过验证后切换生效，使 /name 开始提供新数据库的服务。新版本增加了规范化的 API，因此你可以像这样替换现有数据库（或添加新数据库）： curl -X POST \ -H "Authorization: Bearer $API_TOKEN" \ -H "Accept: application/json" \ -F "db=@content.db" \ -F "db_name=content" \ https://your-instance.example.com/-/upload-dbs 这意味着你可以在 GitHub Actions 等环境中构建全新的数据库，并在构建完成后立即将其切换到生产环境中。 标签：datasette
</think>

# datasette-upload-dbs 0.5a0

**日期：** 2026-08-11 20:35 UTC
**链接：** https://simonwillison.net/2026/Aug/11/datasette-upload-dbs/#atom-everything
**标签：** datasette

---

发布：datasette-upload-dbs 0.5a0 这个插件已经存在一段时间了——它允许用户将全新的 SQLite 数据库上传到托管的 Datasette 实例中，此后该数据库便会由该实例提供服务。它还可以用于将数据库原子性地替换为更新的版本。上传的数据库会被保存到文件，经过验证后切换生效，使 /name 开始提供新数据库的服务。新版本增加了规范化的 API，因此你可以像这样替换现有数据库（或添加新数据库）： curl -X POST \ -H "Authorization: Bearer $API_TOKEN" \ -H "Accept: application/json" \ -F "db=@content.db" \ -F "db_name=content" \ https://your-instance.example.com/-/upload-dbs 这意味着你可以在 GitHub Actions 等环境中构建全新的数据库，并在构建完成后立即将其切换到生产环境中。 标签：datasette
