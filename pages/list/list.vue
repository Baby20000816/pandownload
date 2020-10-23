<template>
	<view style="height: 100vh;" class="flex flex-column">
		<view class="flex border-bottom border-light-secondary" style="height: 100rpx;">
			<view
				class="flex-1 flex flex-column align-center justify-center"
				v-for="(item, index) in tabBars"
				:key="index"
				:class="index === tabIndex ? 'text-main' : ''"
				@click="changeTab(index)"
			>
				<text class="font-md">{{ item.name }}</text>
				<text
					style="height: 8rpx; width: 140rpx;"
					class="rounded"
					:class="tabIndex === index ? 'bg-main' : 'bg-white'"
				></text>
			</view>
		</view>

		<swiper
			:duration="250"
			class="flex-1 flex"
			:current="tabIndex"
			@change="changeTab($event.detail.current)"
		></swiper>
		<swiper-item class="flex-1 flex" v-for="(item, index) in tabBars" :key="index">
			<scroll-view scroll-y="true" class="flex-1">
				<view style="height: 60rpx;" class="bg-light flex align-center font-sm px-2 text-muted">
					下载中({{ downing.length }})
				</view>
				<f-list v-for="(item, index) in downing" :key="'i' + index" :item="item" :index="index">
					<view style="height: 70rpx;" class="flex align-center text-main">
						<text class="iconfont icon-zanting"></text>
						<text class="ml-1">{{ item.download }}</text>
					</view>
					<progress slot="bottom" :percent="item.download" activeColor="#009CFF" :stroke-width="4" />
				</f-list>
				<view class="p-2 border-bottom border-light-secondary font text-muted">
					下载完成({{ downed.length }})
				</view>
				<f-list v-for="(item, index) in downed" :key="'d' + index" :item="item" :index="index" :showRight="false"></f-list>
			</scroll-view>
		</swiper-item>
	</view>
</template>

<script>
import fList from '@/components/common/f-list.vue';
export default {
	components: {
		fList
	},
	data() {
		return {
			tabIndex: 0,
			tabBars: [
				{
					name: '下载列表'
				},
				{
					name: '上传列表'
				}
			],
			list: [
				{
					type: 'image',
					name: '风景.jpg',
					data: 'https://pic-go-test.oss-cn-hangzhou.aliyuncs.com/img/53.jpeg',
					create_time: '2019-12-21 09:00',
					download: 100
				},
				{
					type: 'image',
					name: '壁纸.jpg',
					data: 'https://pic-go-test.oss-cn-hangzhou.aliyuncs.com/img/11.jpeg',
					create_time: '2019-12-21 09:00',
					download: 50
				}
			]
		};
	},
	computed: {
		downing() {
			return this.list.filter(item => {
				return item.download < 100;
			});
		},
		downed() {
			return this.list.filter(item => {
				return item.download === 100;
			});
		}
	},
	methods: {
		changeTab(index) {
			this.tabIndex = index;
		}
	}
};
</script>

<style></style>
