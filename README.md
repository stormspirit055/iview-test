## 前要
我选择fork了`iview`源码, 并在其examples中调试, 这样可以在调试vue源码同时也可以调试该框架源码
## 准备工作
1. 
```
yarn install
```
2. 将`webpack.dev.config.js`中`alias`的配置改成如下图所示, 这样可以直接在`vue.esm.js`中调试源码
```js
    alias: {
        iview: '../../src/index',
        vue: 'vue/dist/vue.esm.js'
    }
```
3. 启动项目进入调试页面, 点击上方`form`, 进入业务调试页面
```
npm run dev
```
## 问题具体描述

#### 必现业务场景简要描述
```
<Form>
    <radiogroup v-modal='switch'>
        xxxx
    </radiogroup> // 控制开关
    <FormItem label='a' prop='a' required v-if='switch' />
    <FormItem label='b' />
    <FormItem label='c' prop='c' v-if='switch' />
</Form>
```
将`switch`调至`false`, 控制台稳定出现如下报错
> 稳定复现的核心是如代码所示, 三个formItem组件依次排序, a有required和v-if属性, b无属性, c有v-if属性, 打乱顺序则不会复现bug

![错误1](https://pic3.zhimg.com/80/v2-f88babfd04412ab61374ad2073400cf8_720w.jpg?source=1940ef5c)


### 目前我碰到的疑惑
* 调试代码
1. 在`Vue.prototype.$watch`中加入`watcher.expression === 'required' && console.log(watcher)`
2. 在`Watcher.prototype.run`中加入`this.expression === 'required' && console.log(this)`

前者会在控制台依次打出id为33,43,53的`Watcher`, 且其对应的vm属性中的label分别为a,b,c
然后点击radio控件, 切换switch属性
此时, 后者会在控制台打出id为33的`Watcher`, 但此时, 其vm属性中的label为`b`
显然id为33的`Watcher`中vm属性进行了替换
于是我猜测, 是在某一步骤又重新赋值了吗?为了验证该猜想, 我做了如下操作
我的方案是这样的:
```js
const obj = {
    a: {
        b: 1
    }
}
let temp = obj.a
obj.a = {
    c: 1
}
console.log(obj.a === temp) //false
```
我想通过一个全局变量保存住调试代码步骤1中id为33的`Watcher`的vm引用地址, 然后在调试步骤2中去对比此时的`this.vm`,因为若是id为33Watcher中的vm进行了重新赋值, 那两者肯定不相等, 可事实是两者相等, 所以我就卡住了
所以我的问题就是id为33`Watcher`实例中的vm为什么会发生改变, 且看上去好像不是进行重新赋值, 而是该引用指向内存里的数据发生了改变...

