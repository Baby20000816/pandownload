<template>
	<view>
		<!-- 自定义导航栏 -->
		<nav-bar>
			<template v-if="checkCount === 0">
				<text slot="left" class="font-md ml-3">首页</text>
				<template slot="right">
					<view
						class="flex align-center justify-center bg-light rounded-circle mr-3"
						style="width: 60rpx; height: 60rpx;"
					>
						<text class="iconfont icon-zengjia"></text>
					</view>
					<view
						class="flex align-center justify-center bg-light rounded-circle mr-3"
						style="width: 60rpx;height: 60rpx;"
					>
						<text class="iconfont icon-gengduo"></text>
					</view>
				</template>
			</template>
			<template v-else>
				<view slot='left' class="font-md ml-3 text-primary" @click="handleCheckAll(false)">取消</view>
				<text class="font-md font-weight-bold">已选中{{checkCount}}个</text>
				<view slot='right' class="font-md mr-3 text-primary"  @click="handleCheckAll(true)">全选</view>
			</template>
		</nav-bar>
		<view class="px-3 py-2">
			<view class="position-relative">
				<view
					class="flex align-center justify-center text-light-muted"
					style="width: 70rpx;height: 70rpx;position: absolute;top: 0;left: 0;"
				>
					<text class="iconfont icon-sousuo"></text>
				</view>
				<!-- <uni-search-bar :radius="20"  placeholder="搜索网盘文件" @confirm="search" cancelButton="none"></uni-search-bar> -->
				<input
					style="height: 70rpx;padding-left: 70rpx;"
					type="text"
					class="bg-lsight font-md rounded-circle"
					placeholder="搜索网盘文件"
				/>
			</view>
		</view>
		<view>
			<fList v-for="(item, index) in lists" :key="index" :item="item" :index="index" @select="select"></fList>
		</view>
	</view>
</template>
<script>
import navBar from '@/components/common/nav-bar.vue';
import uniSearchBar from '@/components/uni-search-bar/uni-search-bar.vue';
import fList from '@/components/common/f-list.vue';
export default {
	components: {
		navBar,
		uniSearchBar,
		fList
	},
	data() {
		return {
			title: 'Hello',
			lists: []
		};
	},
	onLoad() {
		this.getShare();
	},
	computed: {
		checkList() {
			return this.lists.filter(item => item.checked);
		},
		checkCount() {
			return this.checkList.length;
		}
	},
	methods: {
		getShare() {
			uni.request({
				url: 'http://127.0.0.1:7001/list',
				method: 'GET',
				success: res => {
					this.lists = res.data.data;
					// console.log(this.lists)
				},
				fail: function(err) {
					uni.showToast({
						title: '请求失败'
					});
				}
			});
		},
		select(e) {
			this.lists[e.index].checked = e.value;
		},
		handleCheckAll(checked){
			this.lists.forEach(item=>{
				item.checked = checked;
			})
		}
	}
};
</script>

<style>
.content {
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;
}

.logo {
	height: 200rpx;
	width: 200rpx;
	margin-top: 200rpx;
	margin-left: auto;
	margin-right: auto;
	margin-bottom: 50rpx;
}

.text-area {
	display: flex;
	justify-content: center;
}

.title {
	font-size: 36rpx;
	color: #8f8f94;
}

input::-webkit-input-placeholder {
	position: relative;
	left: 20px;
}
</style>
