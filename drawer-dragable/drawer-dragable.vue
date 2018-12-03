<template>
	<view :animation="animationData" class="uni-drawer"  :class="{'uni-drawer-visible':visible,'uni-drawer-right':rightMode}">
		<view v-if="showMask" class="uni-drawer-mask" @tap="close"></view>
		<view class="uni-drawer-content" @touchstart="updateStart" @touchmove="updateMove" @touchend="updateEnd">
			<slot></slot>
		</view>
	</view>
</template>

<script>
	export default {
		props: {
			/**
			 * 显示状态
			 */
			visible: {
				type: Boolean,
				default: false
			},
			/**
			 * 显示模式（左、右），只在初始化生效
			 */
			mode: String,
			/**
			 * 蒙层显示状态
			 */
			mask: {
				type: [Boolean, String],
				default: true
			}
		},
		data() {
			return {
				rightMode: false,
				animationData: {},
				start: 0,
				scale: 3,
				count: 10
			}
		},
		computed: {
			showMask() {
				return String(this.mask) === 'true'
			}
		},
		created() {
			this.rightMode = this.mode === 'right'
			//#ifdef MP-WEIXIN
			this.catchtouchmove = true
			//#endif
			this.animation = uni.createAnimation();
			this.screenWidth = uni.getSystemInfoSync().windowWidth

		},
		methods: {
			close() {
				this.$emit('close')
			},
			updateStart: function(e) {
				this.start = e.touches[0].pageX;
				this.scale = this.screenWidth / (this.screenWidth - this.start)
			},
			updateMove: function(e) {

				this.tx = this.scale * (e.touches[0].pageX - this.start);
				if (this.tx > 0) {
					this.animation.translateX(this.tx).step({
						duration: 100
					})
					this.animationData = this.animation.export()
				}

			},
			updateEnd: function() {
				if(this.tx>100){
					this.close()
				}
				this.animation.translateX(0).step()
				this.animationData = this.animation.export()

			}
		}
	}
</script>

<style scoped>
	.uni-drawer {
		display: block;
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		overflow: hidden;
		visibility: hidden;
		z-index: 99;
	}

	.uni-drawer>.uni-drawer-mask {
		display: block;
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background: rgba(0, 0, 0, 0.4);
	}

	.uni-drawer>.uni-drawer-content {
		display: block;
		position: absolute;
		top: 0;
		left: 0;
		width: 61.8%;
		height: 100%;
		background: #FFFFFF;
		transition: all 0.3s ease-out;
		transform: translatex(-100%);
	}

	.uni-drawer.uni-drawer-right>.uni-drawer-content {
		left: auto;
		right: 0;
		transform: translatex(100%);
	}

	.uni-drawer.uni-drawer-visible {
		visibility: visible;
	}

	.uni-drawer.uni-drawer-visible>.uni-drawer-mask {
		display: block;
	}

	.uni-drawer.uni-drawer-visible>.uni-drawer-content {
		transform: translatex(0);
	}
</style>
