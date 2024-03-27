<template>
	<view :class="open?'mask':'mask-hidden'" @click="closeOpen" :style="{
				background:maskCloseBg,
				left:!show?'1000000px':'0px'
			}">

	</view>
</template>

<script>
	export default {
		data() {
			return {
				show: false
			}
		},

		props: {
			maskCloseBg: {
				type: String,
			},
			open: {
				type: Boolean,
				default: false
			}
		},
		mounted() {
			// this.show = this.open
		},
		watch: {
			open() {
				if (this.open) {
					this.show = true
				} else {
					setTimeout(() => {
						this.show = false
					}, 300)
				}

			}
		},
		methods:{
			closeOpen(){
				this.$emit('close')
			}
		}
	}
</script>

<style scoped>
	.mask {
		height: 100vh;
		width: 750rpx;
		position: fixed;
		z-index: 2;
		top: 0;
		left: 0;
		opacity: 1;
		transition: opacity .3s linear;
	}

	.mask-hidden {
		height: 100vh;
		width: 750rpx;
		position: fixed;
		z-index: 2;
		top: 0;
		left: 0;
		opacity: 0;
		transition: opacity .3s linear;
	}
</style>