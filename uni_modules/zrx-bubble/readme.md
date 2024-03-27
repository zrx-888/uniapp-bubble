## props

<table class="table table-bordered table-striped">
 <tr>														
  <thead>
    <tr>
      <th >参数</th>
      <th >类型</th>
      <th >默认值</th>
      <th>描述</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>position</th>
      <th>string</th>
      <th>bottom</th>
      <th>可选bottom top left right</th>
    </tr>
	<tr>
	  <th>radius</th>
	  <th>string</th>
	  <th>10rpx</th>
	  <th>圆角</th>
	</tr>
	<tr>
	  <th>bgColor</th>
	  <th>string</th>
	  <th>#fff</th>
	  <th>背景颜色</th>
	</tr>
	<tr>
	  <th>maskClose</th>
	  <th>Boolean</th>
	  <th>true</th>
	  <th>点击遮罩层是否关闭</th>
	</tr>
	<tr>
	  <th>maskCloseBg</th>
	  <th>string</th>
	  <th>transparent</th>
	  <th>遮罩层的背景颜色</th>
	</tr> 
	<tr>
	  <th>open</th>
	  <th>Boolean|null</th>
	  <th>null</th>
	  <th>1.为null时 点击弹窗 2. 传入true false 自定义控制显示隐藏</th>
	</tr>
	<tr>
	  <th>change</th>
	  <th>(e:Boolean)=>{}</th>
	  <th></th>
	  <th>onChange 显示隐藏事件 </th>
	</tr>
	<tr>
	  <th>onClose</th>
	  <th>()=>{}</th>
	  <th></th>
	  <th>在传入 open时关闭事件 需要吧传入的open 改为false</th>
	</tr>
  </tbody>
</table>


## Ref

数据加载慢的情况下使用ref 初始化一下
this.$refs.bubbleRef.init()


## 正确写法
```js
   <div style="margin-left:100px">
	   <bubble position="bottom" >
			<template v-slot:tag>
				 <image style="width: 77px;height: 77px;" src="/static/logo.png"></image>
			</template>
			<template v-slot:content>
				<view>
						 内容asaaas
				</view>
			 </template>
   	 </bubble>
   </div>
```


## 错误写法
```js
   <bubble position="bottom" >
				<template v-slot:tag>
				                      这里不要 padding margin 类型的位置css
					<image style="width: 77px;height: 77px;margin-left:100px" src="/static/logo.png"></image>
				</template>
				<template v-slot:content>
					<view>
						 内容asaaas
					</view>
		 </template>
	 </bubble>
```