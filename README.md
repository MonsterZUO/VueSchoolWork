# VueSchoolWork

## 作品主题以及作品内容的设计

主题: 积分系统

#### 内容

1.  通过`积分球`获取积分, 积分可以兑换优惠券和周边商品, 还可以捐赠积分用于慈善活动
2.  通过`聚宝盆`可以消耗积分进行抽奖获得优惠券和周边商品
3.  可在`背包`页面查看自己拥有的优惠券或者周边商品
4.  可在`藏宝阁`页面`出售`和`购买`优惠券和周边商品

## 页面设计和实现

### 主页 `Home.vue`

#### 设计

上部分为积分球和积分显示板块

中部分为积分使用板块

下部分为底部栏

#### 实现

##### 上部分

基本通过css实现.

基础代码

```css
/*构造一个圆*/
width: 50px;
height: 50px;
border-radius: 50%;
```

积分显示使用了3个同心圆,通过最外层圆的绝对定位和对子元素的flex布局实现同心圆效果

积分球使用了一个小圆,通过绝对定位摆放位置.

##### 中部分

调用`vant`的`Tab标签页`组件写了一个`BwtonTabs`组件用于展示多个标签页

###### 优惠券标签页

调用`vant`的`Swipe 轮播`组件写了一个`bwton-post`组件用于轮播图

调用`vant`的`Grid 宫格`组件写了一个`bwton-discount`组件用于展示优惠券

###### 公益标签页

调用`vant`的`NoticeBar 通知栏`和`List 列表`组件

###### 周边标签页

调用之前的`bwton-post`用于轮播图

调用`vant`的`List 列表`和`Card 商品卡片`组件写了一个`bwton-around`组件用于展示周边商品

###### 优惠券标签页和周边标签页的兑换功能

基础代码

```js
Dialog.confirm({
  title: '标题',
  message: '弹窗内容',
})
  .then(() => {
    // on confirm
  })
  .catch(() => {
    // on cancel
  });
```

在组件内调用`vant`的`Dialog 弹出框`组件用于兑换的确认以及通过vuex在背包的数据中添加商品

### 聚宝盆 `Cornucopia.vue`

#### 设计

聚宝盆上方会有积分显示板块

底部是投入积分的按钮以及抽奖按钮

定时会弹出奇遇页面

#### 实现

`vant`组件:`Overlay 遮罩层`, `Dialog 弹出框`组件

###### 积分数据同步

在`computed`中定义每级最大积分和积分显示文本

在每次投入积分确认以后会判断当前积分是否为负数或者超过当前等级的最大积分—>调整当前积分和当前等级—>调整显示文本

### 背包 `Bag.vue`

#### 设计

显示背包里的商品

#### 实现

调用`vant`的`Grid 宫格`组件展示写在vuex中的背包数据

### 藏宝阁 `Shop.vue`

#### 设计

分为`我要买`和`我要卖`标签页用于显示写在vuex中的两个列表数据

每条记录点击以后可进行进一步的购买或者修改出售价格操作

#### 实现

调用`vant`的`Tab标签页`组件用于展示两个标签页

点击每条记录之后通过调用`vant`的`Popup 弹出层`组件弹出操作框

确认购买和下架操作都会通过vuex对数据进行增删来同步背包页面的展示数据.

## 涉及框架

vue+vuex+vue-router

移动端Vue组件库: vant

## 过渡动画

### 主页积分球点击后消失动画

使用vue的`transition`组件.使用典型的消失动画

```vue
<template>
	<transition name="fade">
		<div class="ball" v-if="show1" @click.stop="handleBall1">
			<span class="score">+{{score1}}</span>
		</div>
	</transition>
</template>


/*通过css即可实现*/
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
```
