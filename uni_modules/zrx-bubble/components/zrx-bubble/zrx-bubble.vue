<template>
	<view class="bubble-box" :style="{
		overflow:show?'':'hidden'
	}">

		<view class="">
			<view class="bubble-open" @click="openBubble">
				<slot name="tag"></slot>
			</view>
		</view>
		<view :class="showTransition?'transition-open':'transition-hidden'">
			<view class="bubble-content-popover" :style="{
					left:bubbleStyle.left,
					top:bubbleStyle.top,
					bottom:bubbleStyle.bottom,
					right:bubbleStyle.right,
					transform:bubbleStyle.transform
				}">
				<view class="bubble-content">
					<view class="arr" :style="{
				left:arrStyle.left,
				top:arrStyle.top,
				bottom:arrStyle.bottom,
				right:arrStyle.right,
				transform:arrStyle.transform,
				borderBottomColor:bgColor
			}"></view>
					<view :style="{background:bgColor,borderRadius:radius}" class="bubble-content-box">
						<slot name="content"></slot>
					</view>
				</view>
			</view>
		</view>
		<b-mask :open="showTransition&&show" :maskCloseBg="maskCloseBg" @close="close" v-if="maskClose" />
	</view>
</template>
<script>
	import bMask from './mask.vue';
	/**
	 * position位置  bottom top left right
	 * open 传 true false 自定义控制显示隐藏
	 * maskClose 点击空白是否关闭
	 * onClick 显示隐藏事件
	 * onClose 在传入 open使 关闭事件 需要吧传入的open 改为false
	 */
	export default {
		components: {
			bMask
		},
		props: {
			position: {
				type: String,
				default: "bottom"
			},
			open: {
				type: Boolean,
				default: null
			},
			bgColor: {
				type: String,
				default: "#fff"
			},
			radius: {
				type: String,
				default: "10rpx"
			},
			maskClose: {
				type: Boolean,
				default: true
			},
			maskCloseBg: {
				type: String,
				default: "transparent"
			}
		},
		data() {
			return {
				spacing: 10,
				showTransition: false,
				show: false,
				bubbleBoxSize: {
					bottom: 0,
					height: 0,
					left: 0,
					right: 0,
					width: 0,
					top: 0,
				},
				bubbleContentSize: {
					bottom: 0,
					height: 0,
					left: 0,
					right: 0,
					width: 0,
					top: 0,
				},
				arrStyle: {},
				bubbleStyle: {},
				windowWidth: uni.getSystemInfoSync().windowWidth
			}
		},
		mounted() {
			this.initBubblePosition();
			this.initBubbleContentSize();
		},
		watch: {
			open(e) {
				this.showTransition = this.open
				if (this.showTransition) {
					this.show = true
				} else {
					setTimeout(() => {
						this.show = false
					}, 100)
				}
			}

		},
		methods: {
			bubbleContentStyle() {
				if (!this.show) {
					return {}
				}
				let style = {}
				let arrStyle = {}
				const left = (this.bubbleBoxSize.width - this.bubbleContentSize.width) / 2
				const reacLeft = this.bubbleBoxSize.left
				const reacRight = this.windowWidth - this.bubbleBoxSize.right
				const residueWidth = (this.bubbleContentSize.width - this.bubbleBoxSize.width) / 2 + 10
				if (reacRight > residueWidth && reacLeft > residueWidth) {
					style.left = `${left}px`
				} else {
					if (reacLeft < reacRight) {
						style.left = this.bubbleBoxSize.left <= 10 ? "10px" : `-${this.bubbleBoxSize.left-10}px`
					} else {
						style.right = reacRight <= 10 ? "10px" : `-${reacRight-10}px`
						style.left = `auto`
					}

				}
				switch (this.position) {
					case 'bottom':
						if (reacRight > residueWidth && reacLeft > residueWidth) {
							arrStyle = {
								top: `-30rpx`,
								left: `50%`,
								transform: "translateX(-50%)",
							}
						} else {
							if (reacLeft < reacRight) {
								arrStyle = {
									top: `-30rpx`,
									left: `${this.bubbleBoxSize.left+this.bubbleBoxSize.width/2-10-this.rpxToPx(17)}px`,
								}
							} else {
								arrStyle = {
									top: `-30rpx`,
									right: `${reacRight+this.bubbleBoxSize.width/2-10-this.rpxToPx(17)}px`,
								}
							}
						}
						style.top = `${this.bubbleBoxSize.height + this.spacing + 5}px`
						break;
					case 'left':
						if (this.bubbleBoxSize.left < this.bubbleContentSize.width + this.spacing + 20) {
							const left = this.bubbleContentSize.width - this.bubbleBoxSize.left
							arrStyle = {
								right: `-30rpx`,
								top: '50%',
								transform: "translateY(-50%)  rotate(90deg)",
							}
							style = {
								top: '50%',
								transform: "translateY(-50%)",
								left: `-${this.bubbleContentSize.width-left-this.spacing}px`
							}
						} else {
							arrStyle = {
								right: `-30rpx`,
								top: '50%',
								transform: "translateY(-50%)  rotate(90deg)",
							}
							style = {
								top: '50%',
								transform: "translateY(-50%)",
								left: `-${this.bubbleContentSize.width+this.spacing+5}px`
							}
						}

						break;
					case 'top':
						if (reacRight > residueWidth && reacLeft > residueWidth) {
							arrStyle = {
								bottom: `-30rpx`,
								left: `50%`,
								transform: "translateX(-50%) rotate(180deg)",
							}
						} else {
							if (reacLeft < reacRight) {
								arrStyle = {
									bottom: `-30rpx`,
									left: `${this.bubbleBoxSize.left+this.bubbleBoxSize.width/2-10-this.rpxToPx(17)}px`,
									transform: "rotate(180deg)",
								}
							} else {
								arrStyle = {
									bottom: `-30rpx`,
									right: `${reacRight+this.bubbleBoxSize.width/2-10-this.rpxToPx(17)}px`,
									transform: "rotate(180deg)",
								}
							}
						}
						style.top = `-${this.bubbleContentSize.height + this.spacing+5}px`
						break;
					case 'right':
						const rightPx = this.windowWidth - this.bubbleBoxSize.right
						if (rightPx < this.bubbleContentSize.width + this.spacing + 20) {
							const left = this.bubbleContentSize.width - rightPx
							arrStyle = {
								left: `-30rpx`,
								top: '50%',
								transform: "translateY(-50%)  rotate(270deg)",
							}
							style = {
								top: '50%',
								transform: "translateY(-50%)",
								left: `${this.bubbleBoxSize.width-left-10}px`,
							}
						} else {
							arrStyle = {
								left: `-30rpx`,
								top: '50%',
								transform: "translateY(-50%)  rotate(270deg)",
							}
							style = {
								top: '50%',
								transform: "translateY(-50%)",
								left: `${this.bubbleBoxSize.width+this.spacing+5}px`
							}
						}

						break;
					default:
						break;
				}
				this.arrStyle = arrStyle
				this.bubbleStyle = style
				console.log(this.bubbleStyle);
				console.log(this.arrStyle);
			},
			openBubble() {
				if (this.open === null) {
					this.showTransition = !this.showTransition
					if (this.showTransition) {
						this.show = true
						this.bubbleContentStyle()
					} else {
						setTimeout(() => {
							this.show = false
						}, 100)
					}

					this.$emit('change', this.showTransition)
				}
			},
			close() {
				if (!this.maskClose) return
				if (this.open === null) {
					if (this.showTransition) {
						this.openBubble()
					}
				} else {
					if (this.showTransition) {
						this.$emit('onClose')
					}
				}
			},
			init() {
				this.initBubbleContentSize()
				this.initBubblePosition()
			},
			rpxToPx(rpx) {
				const windowWidth = uni.getSystemInfoSync().windowWidth
				return (windowWidth * rpx) / 750
			},

			initBubbleContentSize() {
				let view = uni.createSelectorQuery().in(this).select(".bubble-content-popover");
				view.boundingClientRect(data => {
					this.bubbleContentSize = data
				}).exec();
			},
			initBubblePosition() {
				let view = uni.createSelectorQuery().in(this).select(".bubble-box");
				view.boundingClientRect(data => {
					this.bubbleBoxSize = data
				}).exec();
			}
		}
	}
</script>

<style scoped>
	.bubble-box {
		position: relative;
		display: inline-block;
	}

	.bubble-content-box {
		box-shadow: 0 0 30rpx rgba(0, 0, 0, 0.15);
		overflow: hidden;
	}

	.bubble-open {
		z-index: 4;
		position: relative;
	}

	.bubble-content-popover {
		position: absolute;
		border-radius: 10rpx;
		z-index: 5;
		left: 10000000px;

		z-index: 6;
	}


	.bubble-content {
		position: relative;
	}

	.arr {
		position: absolute;
		border: 17rpx solid;
		border-color: transparent transparent #fff transparent;
	}

	.transition-open {
		opacity: 1;
		transition: opacity .1s linear;
	}

	.transition-hidden {
		opacity: 0;
		transition: opacity .1s linear;
	}
</style>