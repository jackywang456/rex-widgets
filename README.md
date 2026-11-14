# rex-widgets

佳俊（[jackywang456](https://github.com/jackywang456)）的 ForwardWidget 模块库。

每个模块是 `widgets/` 里的一个 JavaScript 文件，供 rex App 使用。写法遵循官方项目 [ForwardWidgets](https://github.com/InchStudio/ForwardWidgets) 的 `WidgetMetadata`、`loadList`、`loadDetail` 和 `search` 约定。

## 目录

```
widgets/
  <模块名>.js    一个文件一个模块
```

`widgets/.gitkeep` 用来让空目录被 Git 跟踪。

## 新增模块

1. 在 `widgets/` 下新建 `<模块名>.js`。
2. 文件以 `WidgetMetadata` 开头，写明 `id`、`title`、`description`、`author`、`version` 和 `modules`。需要搜索时再加上可选的 `search`，其 `functionName` 用 `search`。
3. 实现与元数据对应的函数：
   - `loadList(params)`：列表，返回视频条目数组。
   - `loadDetail(link)`：顶层详情函数，用列表条目里的 `link` 补全详情，不写进 `modules`。
   - `search(params)`：对应 `WidgetMetadata.search`。
4. 参数类型、`VideoItem` 字段和其它模块类型以官方文档为准：<https://github.com/InchStudio/ForwardWidgets>

```javascript
var WidgetMetadata = {
  id: "example.id",
  title: "模块标题",
  description: "模块说明",
  author: "佳俊",
  version: "1.0.0",
  requiredVersion: "0.0.1",
  modules: [
    {
      title: "列表",
      functionName: "loadList",
      params: [],
    },
  ],
  // 不需要搜索时去掉 search
  search: {
    title: "搜索",
    functionName: "search",
    params: [],
  },
};

async function loadList(params) {
  return [];
}

async function loadDetail(link) {
  return null;
}

async function search(params) {
  return [];
}
```

上面只标出入口形状。请求、解析和返回字段按官方文档来写。

## 索引

官方仓库的 [`generate-widget-index.js`](https://github.com/InchStudio/ForwardWidgets/blob/master/generate-widget-index.js) 会扫描 `widgets/*.js`，读出每个文件的 `WidgetMetadata`，生成 `forward-widgets.fwd`（一份 JSON 索引，包含标题、说明，以及各模块的 id、标题、版本和脚本地址）。

本仓库还没有这份脚本，也还没有 `forward-widgets.fwd`。以后模块多了，可以按该脚本的做法在本地生成索引。官方脚本里的脚本地址指向 `https://assets.vvebo.vip/scripts/`，给 rex 用时改成下面的 GitHub raw 地址。

## 导入 rex

rex 用 GitHub 上单个文件的 raw 地址导入模块：

```
https://raw.githubusercontent.com/jackywang456/rex-widgets/main/widgets/<文件名>.js
```

把 `<文件名>.js` 换成 `widgets/` 里的实际文件名。地址里的分支是 `main`。
