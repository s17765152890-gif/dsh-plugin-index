# DSH 插件市场索引（s17765152890-gif）

一份符合 **插件市场索引格式 1** 的 JSON 索引，供 DSH 的「插件 → 发现」读取。

## 添加索引源

在 DSH 侧栏打开 **插件 → 发现 → 添加索引源**，填入：

```
https://raw.githubusercontent.com/s17765152890-gif/dsh-plugin-index/main/index.json
```

添加后即可在搜索框按名称检索并一键安装。

## 收录的插件

| 包名 | 标题 | 版本 | 仓库 |
|---|---|---|---|
| `dsh-usage-quota` | 用量与思考强度 | 1.0.2 | https://github.com/s17765152890-gif/dsh-usage-quota |
| `dsh-model-select-test` | 模型测试与目录刷新 | 0.2.0 | https://github.com/s17765152890-gif/dsh-model-select-test |
| `dsh-opencode-go-catalog` | OpenCode Go 目录同步 | 0.3.0 | https://github.com/s17765152890-gif/dsh-opencode-go-catalog |

三个条目都使用 **git 源**（`source.type = "git"`，固定到提交 SHA），因此不依赖 npm 发布；安装时 DSH 会用
`git+https://github.com/<owner>/<repo>.git#<commit>` 作为安装参数。

## 格式说明

索引遵循 DSH 内置校验器 `@deepseek-ai/dsh-plugin-market-index` 的格式 1：

```json
{
  "format": 1,
  "title": { "en": "...", "zh": "..." },
  "metadata": { "categories": {}, "labels": {}, "sortKeys": {} },
  "plugins": [
    {
      "name": "npm-compatible-name",
      "title": { "en": "...", "zh": "..." },
      "description": { "en": "...", "zh": "..." },
      "author": "...",
      "homepage": "https://...",
      "repository": { "url": "https://github.com/owner/repo" },
      "metadata": { "category": "ui", "keywords": [], "labels": [], "sortKeys": {} },
      "versions": [
        {
          "version": "1.0.0",
          "publishTimestamp": 1794067200,
          "source": {
            "type": "git",
            "url": "https://github.com/owner/repo",
            "commit": "<40-hex-sha>"
          },
          "engines": { "dsh": ">=0.2.0-rc.2" }
        }
      ]
    }
  ]
}
```

要点：

- `title` / `description` 可写字符串，或按语言 id 索引的对象且**必须包含 `en`**；
- `metadata.labels` / `category` 必须引用索引 `metadata` 中声明过的 id；
- `source.type` 支持 `"npm"`（配 `version` 与可选 `registry`）和 `"git"`（必须给 40 位小写 commit）；
- `engines.dsh` 为 SemVer 范围，DSH 用它判断条目与当前运行时是否兼容。

## 更新方式

发布新版本后，把对应条目的 `versions[0]` 改成新的 `version` / `publishTimestamp` / `commit`，提交即可。
DSH 会按索引 URL 缓存，刷新「发现」标签页后生效。

## 许可证

MIT
