<template>
	<view :animation="animationData" class="uni-drawer" :class="{'uni-drawer-visible':option.visible,'uni-drawer-right':option.rightMode}">
		<!-- #ifndef APP-PLUS -->
		<view v-if="option.mask" class="uni-drawer-mask" @tap.stop="close" @touchmove.stop="stopref" @touchend="stopref"></view>
		<!-- #endif -->
		<!-- #ifdef APP-PLUS -->
		<view v-if="option.mask" class="uni-drawer-mask" @tap.stop="close"></view>
		<!-- #endif -->
		<view class="uni-drawer-content" @touchstart.stop="updateStart" @touchmove.stop="updateMove" @touchend.stop="updateEnd"
		 :style="{width:option.width+'%'}">
			<slot></slot>
		</view>
	</view>
</template>

<script>
	export default {
		name:'hj-dragable-drawer',
		props: {
			/**
			 * 显示状态
			 */
			options: Object
		},
		data() {
			return {
				def: {
					visible: false,
					rightMode: true,
					mask: true,
					width: 60
				},
				animationData: {}
			}
		},
		computed: {
			option: function() {
				return { ...this.def,
					...this.options
				}
			}
		},
		created() {
			// #ifdef APP-PLUS
			const pages = getCurrentPages();
			const page = pages[pages.length - 1];
			const currentWebview = page.$getAppWebview();
			currentWebview.setStyle({
				pullToRefresh: {
					support: false
				}
			});
			// #endif
			this.animation = uni.createAnimation();
			this.screenWidth = uni.getSystemInfoSync().windowWidth;
		},
		methods: {
			close() {
				this.options.visible = false;
			},
			updateStart: function(e) {
				this.start = e.touches[0].pageX;
				this.scale = this.screenWidth / (this.screenWidth - this.start)
			},
			updateMove: function(e) {
				// #ifndef APP-PLUS
				this.stopref()
				// #endif
				this.tx = this.scale * (e.touches[0].pageX - this.start);
				if (this.option.rightMode) {
					if (this.tx > 0) {
						this.animation.translateX(this.tx).step({
							duration: 100
						})
					}
				} else {
					if (this.tx < 0) {
						this.animation.translateX(this.tx).step({
							duration: 100
						})
					}
				}
				this.animationData = this.animation.export()
			},
			updateEnd: function() {
				// #ifndef APP-PLUS
				this.stopref()
				// #endif

				if (this.option.rightMode) {
					if (this.tx > 100) {
						this.close()
					}
				} else {
					if (this.tx < -80) {
						this.close()
					}
				}
				this.animation.translateX(0).step()
				this.animationData = this.animation.export()
			},
			// #ifndef APP-PLUS
			stopref: uni.stopPullDownRefresh,
			// #endif
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
