<template>
	<!-- 封装自定义的全局弹出层，通过ref引用dom元素 -->
	<uni-popup ref="dialog">
		<!-- 弹出层最外层容器样式 -->
		<view style="width: 600rpx;" class="bg-white rounded">
			<view class="flex align-center justify-center font-weight-bold border-bottom border-light-secondary" style="height: 100rpx;">
				{{title}}
			</view>
			<view class="flex border-top border-light-secondary p-3"><slot></slot></view>
			<view class="flex border-top border-light-secondary" style="height: 100rpx;">
				<view class="flex-1 text-muted flex align-center justify-center" @tap="cancel">{{cancelText}}</view>
				<view class="flex-1 text-main flex align-center justify-center" @tap="confirm">{{confirmText}}</view>
			</view>
		</view>
	</uni-popup>
</template>

<script>
	import uniPopup from '@/components/uni-ui/uni-popup/uni-popup.vue';
	export default {
		components:{
			uniPopup
		},
		props:{
			title:{
				type: String,
				default: '提示'
			},
			cancelText:{
				type: String,
				default: '取消'
			},
			confirmText:{
				type: String,
				default: '确定'
			}
		},
		data(){
			return{
				callback:false
			};
		},
		methods:{
			open(callback=false){
				this.callback = callback;
				this.$refs.dialog.open();
			},
			cancel(){
				this.$emit('cancel');
				this.$refs.dialog.close();
			},
			confirm(){
				if(typeof this.callback ==='function'){
					this.callback(()=>{
						this.cancel();
					});
				}else{
					this.$emit('confirm');
					this.cancel();
				}
			}
		}
	}
</script>

<style>
</style>
