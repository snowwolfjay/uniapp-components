<template>
	<view class="uni-uploader" :class="bgstyle">
		<view class="uni-uploader-head">
			<view class="uni-uploader-info">{{tips}}</view>
			<view class="uni-uploader-info">{{currentNum}}/{{maxNum}}</view>
		</view>
		<view class="uni-uploader-body">
			<view class="uni-uploader__files" >
				<block v-if="imageList.length>0">
					<view class="uni-uploader__file" v-for="(image,index) in imageList" :key="index">
						<image class="uni-uploader__img" :src="imageList[index]" @tap="previewImage" :data-index="index" @longpress="changeImg"></image>
						<label class="img-del-icon" :data-index="index" @tap="delImg">X</label>
					</view>
				</block>
				<view class="uni-uploader__input-box" v-if="notfull">
					<view class="uni-uploader__input" @tap="chooseImage"></view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		props: {
			/**
			 * 提示
			 */
			tips: {
				type: String,
				default: ''
			},
			/**
			 * 计数
			 */
			maxNum: {
				type: Number,
				default: 3
			},
			// 图片列表
			imageList: [String],
			bgstyle: {
				type: String,
				default: 'light'
			},
			camera:{
				type: Boolean,
				default: false
			},
			album:{
				type: Boolean,
				default: false
			},
			compressed:{
				type: Boolean,
				default: false
			},
			original:{
				type: Boolean,
				default: false
			}
		},
		computed: {
			notfull() {
				return this.imageList.length < this.maxNum
			},
			currentNum: function() {
				return this.imageList.length;
			},
			sourceType:function () {
				if(this.camera) return ['camera'];
				if(this.album) return ['album'];
				return ['camera','album']
			},
			sizeType:function () {
				if(this.original) return ['original'];
				if(this.compressed) return ['compressed'];
				return ['original','compressed']
			}
		},
		methods: {
			onClick: function() {
				this.$emit('click')
			},
			chooseImage: function() {
				uni.chooseImage({
					sourceType: this.sourceType,
					sizeType: this.sizeType,
					count: (this.maxNum-this.currentNum),
					success: (res) => {
						this.imageList.push(...res.tempFilePaths);
					},
				})
			},
			changeImg: function(e) {
				let index = e.target.dataset.index;
				uni.chooseImage({
					sourceType: this.sourceType,
					sizeType: this.sizeType,
					count: 1,
					success: (res) => {
						this.$set(this.imageList, index, res.tempFilePaths[0])
					}
				})
			},
			delImg: function(e) {
				let index = e.target.dataset.index;
				this.imageList.splice(index, 1);
			},
			previewImage: function(e) {
				var index = e.target.dataset.index
				uni.previewImage({
					current: this.imageList[index],
					urls: this.imageList
				})
			}
		}
	}
</script>
<style scoped="">
	.uni-uploader {
		box-sizing: border-box;
		width: 100%;
		flex: 0;
		flex-direction: column;
		padding: 35upx;
	}
	.dark {
		background-color: #555;
	}
	.light {
		background-color: white
	}

	.uni-uploader-head {
		display: flex;
		flex-direction: row;
		justify-content: space-between;
	}

	.uni-uploader-info {
		color: #B2B2B2;
	}

	.uni-uploader-body {
		flex: 1;
		margin-top: 16upx;
		display: flex;
		flex-direction: column;
	}

	.uni-uploader__files {
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
	}

	.uni-uploader__file {
		position: relative;
		margin: 10upx;
		width: 210upx;
		height: 210upx;
	}

	.uni-uploader__img {
		display: block;
		width: 210upx;
		height: 210upx;
	}

	.uni-uploader__input-box {
		position: relative;
		margin: 10upx;
		width: 208upx;
		height: 208upx;
		border: 2upx solid #D9D9D9;
	}

	.uni-uploader__input-box:before,
	.uni-uploader__input-box:after {
		content: " ";
		position: absolute;
		top: 50%;
		left: 50%;
		-webkit-transform: translate(-50%, -50%);
		transform: translate(-50%, -50%);
		background-color: #D9D9D9;
	}

	.uni-uploader__input-box:before {
		width: 4upx;
		height: 79upx;
	}

	.uni-uploader__input-box:after {
		width: 79upx;
		height: 4upx;
	}

	.uni-uploader__input-box:active {
		border-color: #999999;
	}

	.uni-uploader__input-box:active:before,
	.uni-uploader__input-box:active:after {
		background-color: #999999;
	}

	.uni-uploader__input {
		position: absolute;
		z-index: 1;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		opacity: 0;
	}

	.img-del-icon {
		display: inline-block;
		position: absolute;
		background-color: white;
		left: 140upx;
		bottom: 148upx;
		width: 48upx;
		height: 48upx;
		line-height: 48upx;
		text-align: center;
		border-radius: 24upx;
	}
</style>
