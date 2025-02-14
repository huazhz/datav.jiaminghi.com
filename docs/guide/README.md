# 介绍

组件库基于Vue，主要用于构建大屏（**全屏**）数据展示页面即**数据可视化**，具有多种类型组件可供使用：

* **边框**

带有不同边框的容器

* **装饰**

用来点缀页面效果，增加视觉效果

* **图表**

图表组件基于[Charts](http://charts.jiaminghi.com)封装，轻量，易用

* **其他**

飞线图/水位图/轮播表/...

::: tip TIP
建议使用Chrome浏览器。
:::

## 安装

* npm安装

```sh
npm install @jiaminghi/data-view
```

* yarn安装
```sh
yarn add @jiaminghi/data-view
```

## 使用

```js
// 将自动注册所有组件为全局组件
import dataV from '@jiaminghi/data-view'

Vue.use(dataV)
```

## 按需引入

按需引入仅支持基于**ES module**的**tree shaking**，按需引入示例如下：

```js
import { borderBox1 } from '@jiaminghi/data-view'

Vue.use(borderBox1)
```