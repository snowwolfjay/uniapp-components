
##用于图片选择、预览、删除操作。
###标签tag:
####  <image-picker></image-picker>
###属性attributes:
            tips: [String] 文字说明;
			maxNum: [Number] 图片上限，默认3
			// 图片列表
			imageList: [String] 图片地址列表，绑定后可以在父组件操作列表
			bgstyle: [String <'dark'/'light'>] 组件背景 
            图片来源：默认两种，写上某种属性就表示只用此来源
			camera:
			album:
            图片是否压缩：默认两种，写上某种属性就表示只用此来源
			compressed:
			original: