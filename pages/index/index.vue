<template>
	<view class="content">
		<bubble position="bottom" >
			<template v-slot:tag>
				<image class="logo" src="/static/logo.png"></image>
			</template>
			<template v-slot:content>
				<view style="padding: 20px;">
					<view v-for="(item,index) in count"  style="width:250rpx;height:80rpx;" :key="index">
						{{index+1}}
					</view>
				</view>
			</template>
		</bubble>
	</view>
</template>

<script>
	import bubble from '@/uni_modules/zrx-bubble/components/zrx-bubble/zrx-bubble.vue'
	export default {
		components: {
			bubble
		},
		data() {
			return {
				title: 'Hello',
				count:0
			}
		},
		onLoad() {
				this.count=6
		},
		methods: {

		}
	}
</script>

<style>
	.content { 
		 height: 100vh;
		 text-align: center;
		 background: #fff;
		 padding-right:140px;
		 padding-top: 200px;
	}

	.logo {
		height: 140rpx;
		width: 140rpx;
		display: block;
	}
 
</style>