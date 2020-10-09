# 引入element ui

```
npm i element-ui -S
```

在mian.js中引入

```js
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
//在vue脚手架中使用elementui
Vue.use(ElementUI);
```



## Container 布局容器

`<el-container>`：外层容器。当子元素中包含 `<el-header>` 或 `<el-footer>` 时，全部子元素会垂直上下排列，否则会水平左右排列。

`<el-header>`：顶栏容器。

`<el-aside>`：侧边栏容器。

`<el-main>`：主要区域容器。

`<el-footer>`：底栏容器。

```HTML
<el-container>
  <el-header>我是标题</el-header>
  <el-container><el-aside>我是菜单</el-aside></el-container>
  <el-main>我是中心内容</el-main>
  <el-footer>我是页脚</el-footer>
</el-container>
```

