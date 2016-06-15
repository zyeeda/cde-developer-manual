# tree

### 描述

tree 展现为一个树形结构，内部采用目前较流行的 jQuery 插件 zTree 实现。

可以通过下面的方式定义 tree ：

```js
exports.tree = {
	isAsync: true,
	root: '目录'
};
```

其中 `isAsync` 代表是否为异步方式展示； `root` 代表根节点，根节点是不会存入数据库而只用于页面展示的虚拟节点。

其它属性采用 zTree 原生 API ，详细信息请参考 <a href="http://www.ztree.me/v3/api.php" target="_blank">&nbsp;zTree&nbsp;</a> 官方文档。