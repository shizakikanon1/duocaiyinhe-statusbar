# duocaiyinhe-statusbar

从用户提供的「多彩银河」角色卡 JSON 中逐字提取的「状态栏美化」正则内容（星海大学 · 暗紫粉银河风状态栏）。
保留原版的世界行、角色栏、淫能/快感/雌堕/精液条、当前目标、在场人物与五个抽屉分页，没有重新设计界面。

## 文件

- `src/statusbar/index.html`：从卡内正则 `状态栏美化` 提取的完整状态栏文档。只去掉了外层的 ```html 围栏，样式、脚本与结构逐字保留。
- `duocaiyinhe-statusbar-index.html`：发布产物，与 `src` 完全相同（单文件，样式与脚本全部内联，无外部资源）。
- `asset-manifest.json`：来源正则 id、原始替换内容与产物的 SHA-256、处理说明。

## SillyTavern 接入

卡片「状态栏美化」正则的替换内容改为下面这段 loader（保留外层三反引号围栏），把 `<commit>` 换成实际发布提交的完整 SHA：

```html
<body>
<script>
$(function () {
  $('body').load('https://testingcf.jsdelivr.net/gh/shizakikanon1/duocaiyinhe-statusbar@<commit>/duocaiyinhe-statusbar-index.html', function (_response, status) {
    if (status === 'error') {
      $('body').text('状态栏加载失败，请检查网络后重试。');
    }
  });
});
</script>
</body>
```

- 占位符仍是 `<StatusPlaceHolderImpl/>`，placement 仍为 `[2]`，`markdownOnly` 保持开启。
- 运行时依赖酒馆助手 iframe 环境提供的 `$`、`_`、`waitGlobalInitialized`、`eventOn`、`errorCatched`、`getAllVariables`、`substitudeMacros`，以及 MVU 的 `Mvu.events.VARIABLE_UPDATE_ENDED`；没有新增外部依赖。
- 状态栏脚本是内联的 `<script type="module">`，但脚本内没有 `import` / `export` 与顶层 `await`。jQuery 的 HTML 注入会以经典脚本方式执行它；已用 SillyTavern 自带的 jQuery 3.5.1 做过无头浏览器实测，注入后状态栏能正常渲染。

## 维护

改 `src/statusbar/index.html` → 同步 `duocaiyinhe-statusbar-index.html` → 提交 → 用新的完整 commit SHA 更新卡内 loader。
固定完整 SHA 可以避免旧版本 jsdelivr 缓存与分支漂移。

## 边界

仓库只包含状态栏前端本身：不含角色卡、世界书、开场白、首楼界面（星海大学入学登记）、选项栏或任何图片资源。
状态栏读取的变量结构来自角色卡自带的「变量结构」脚本，本仓库不重复收录。
