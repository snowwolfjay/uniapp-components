## 一个可以响应手势操作的抽屉组件
### 标签名: ```<hj-dragable-drawer>```

### 属性：options [Object] 
>#### 是否可见（默认： false ）：visible:Boolean 
>#### 是否右侧显示（默认：true）：rightMode: Boolean 
>#### 是否在空余部分添加遮罩（默认：true）： mask: Boolean 
>#### 抽屉宽度百分比（默认：60）： width: Number 
>#### 注：默认点击遮罩关闭抽屉

### 已测试：微信小程序，安卓APP，H5

### 使用示例
```
<template>
	<view class="content">
		<hj-dragable-drawer :options="options">
			<button>hi</button>
			<text>不建议使用v-for进行列表渲染</text>
		</hj-dragable-drawer>
		<button @tap="open">打开</button>
</view>
</template>
<script>
	import hjDragableDrawer from '../../components/hj/dragable-drawer.vue'
	export default {
		data() {
			return {
				options: {
					visible: false,
					rightMode:false
				}
			};
		},
		components: {
			hjDragableDrawer
		},
		methods: {
			open: function() {
				this.option.visible = true
			}
		}
	}
</script>
```