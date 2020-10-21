<template>
	<view>
		<!-- 自定义导航栏 -->
		<nav-bar>
			<text slot="left" class="font-md ml-3 text-white">首页</text>
			<template slot="right">
				<view
					class="flex align-center justify-center bg-icon rounded-circle mr-3"
					style="width: 60rpx; height: 60rpx;"
				>
					<text class="iconfont icon-zengjia"></text>
				</view>
				<view
					class="flex align-center justify-center bg-icon rounded-circle mr-3"
					style="width: 60rpx;height: 60rpx;"
				>
					<text class="iconfont icon-gengduo"></text>
				</view>
			</template>
		</nav-bar>
		<view class="flex" style="height: 1000px;">
			<view>
				<view class="ml-4" style="width: 700rpx;border-radius: 30rpx;">
					<!-- <text class="iconfont icon-sousuo mr-1" style="color:#a7a39f;"></text>
					<text style="color: #9b9c9d;">搜索网盘文件</text>-->
					<!-- <input /> -->
				</view>
				<uni-search-bar  :radius="20"  placeholder="搜索网盘文件" @confirm="search" cancelButton="none"></uni-search-bar>
			</view>
		</view>
	</view>
</template>

<script>
import navBar from '@/components/common/nav-bar.vue';
import uniSearchBar from '@/components/uni-search-bar/uni-search-bar.vue';
export default {
	components: {
		navBar,
		uniSearchBar
	},
	data() {
		return {
			title: 'Hello'
		};
	},
	onLoad() {},
	methods: {}
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
