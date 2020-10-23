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
						@tap="openAddDialog"
					>
						<text class="iconfont icon-zengjia"></text>
					</view>
					<view
						class="flex align-center justify-center bg-light rounded-circle mr-3"
						style="width: 60rpx;height: 60rpx;"
						@click="openSortDialog"
					>
						<text class="iconfont icon-gengduo"></text>
					</view>
				</template>
			</template>
			<template v-else>
				<view slot="left" class="font-md ml-3 text-primary" @click="handleCheckAll(false)">取消</view>
				<text class="font-md font-weight-bold">已选中{{ checkCount }}个</text>
				<view slot="right" class="font-md mr-3 text-primary" @click="handleCheckAll(true)">全选</view>
			</template>
		</nav-bar>
		<view class="px-3 py-2">
			<view class="position-relative">
				<input
					style="height: 70rpx;padding-left: 70rpx;"
					type="text"
					class="bg-lsight font-md rounded-circle"
					placeholder="搜索网盘文件"
				/>
			</view>
		</view>
		<view>
			<fList
				v-for="(item, index) in lists"
				:key="index"
				:item="item"
				:index="index"
				@select="select"
				@click="doEvent(item)"
			></fList>
		</view>
		<view v-if="checkCount > 0">
			<view class="flex align-stretch bg-primary text-white fixed-bottom">
				<view
					class="flex-1 flex flex-column align-center justify-center"
					style="line-height: 1.5;"
					v-for="(item, index) in actions"
					:key="index"
					hover-class="bg-hover-primary"
					@click="handleBottomEvent(item)"
				>
					<text class="iconfont" :class="item.icon"></text>
					{{ item.name }}
				</view>
			</view>
		</view>

		<!-- 		是否要删除
		<f-dialog ref="dialog">是否删除选中的文件？</f-dialog> -->
		<!-- 是否要删除 -->
		<f-dialog ref="delete">是否删除选中的文件？</f-dialog>
		<f-dialog ref="rename">
			<input
				type="text"
				v-model="renameValue"
				class="flex-1 bg-light rounded px-2"
				style="height: 95rpx;"
				placeholder="重命名"
			/>
		</f-dialog>
		<f-dialog ref="newdir">
			<input
				type="text"
				v-model="newdirname"
				class="flex-1 bg-light rounded px-2"
				style="height: 95rpx;"
				placeholder="新建文件夹名称"
			/>
		</f-dialog>
		<uni-popup ref="add" type="bottom">
			<view class="bg-white flex" style="height: 200rpx;">
				<view
					class="flex-1 flex align-center justify-center flex-column"
					hover-class="bg-light"
					v-for="(item, index) in addList"
					:key="index"
					@tap="handleAddEvent(item)"
				>
					<text
						style="width: 110rpx; height: 110rpx;"
						class="rounded-circle bg-light iconfont flex align-center justify-center"
						:class="item.icon + ' ' + item.color"
					></text>
					<text class="font text-muted">{{ item.name }}</text>
				</view>
			</view>
		</uni-popup>
		<uni-popup ref="sort" type="bottom">
			<view class="bg-white">
				<view v-for="(item,index) in sortOptions" :key="index" class="flex align-center justify-center py-3 font border-bottom border-light-secondary" :class="index===sortIndex?'text-main':'text-dark'" hover-class="bg-light" @click="changeSort(index)">{{item.name}}</view>
			</view>
		</uni-popup>
	</view>
</template>
<script>
import navBar from '@/components/common/nav-bar.vue';
import uniSearchBar from '@/components/uni-search-bar/uni-search-bar.vue';
import fList from '@/components/common/f-list.vue';
import fDialog from '@/components/common/f-dialog.vue';
import uniPopup from '@/components/uni-ui/uni-popup/uni-popup.vue';
export default {
	components: {
		navBar,
		uniSearchBar,
		fList,
		fDialog,
		uniPopup
	},
	data() {
		return {
			title: 'Hello',
			renameValue: '',
			lists: [],
			addList: [
				{
					icon: 'icon-file-b-6',
					color: 'text-success',
					name: '上传图片'
				},
				{
					icon: 'icon-file-b-9',
					color: 'text-primary',
					name: '上传视频'
				},
				{
					icon: 'icon-file-b-8',
					color: 'text-muted',
					name: '上传文件'
				},
				{
					icon: 'icon-file-b-2',
					color: 'text-warning',
					name: '新建文件夹'
				}
			],
			newdirname: '',
			sortIndex: 0,
			sortOptions: [
				{
					name: '按名称排序'
				},
				{
					name: '按时间排序'
				}
			]
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
		},
		actions() {
			if (this.checkCount > 1) {
				return [
					{
						icon: 'icon-xiazai',
						name: '下载'
					},
					{
						icon: 'icon-shanchu',
						name: '删除'
					}
				];
			}
			return [
				{
					icon: 'icon-xiazai',
					name: '下载'
				},
				{
					icon: 'icon-fenxiang-1',
					name: '分享'
				},
				{
					icon: 'icon-shanchu',
					name: '删除'
				},
				{
					icon: 'icon-chongmingming',
					name: '重命名'
				}
			];
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
		changeSort(index){
			this.sortIndex = index;
			this.$refs.sort.close();
		},
		handleCheckAll(checked) {
			this.lists.forEach(item => {
				item.checked = checked;
			});
		},
		// handleBottomEvent(item) {
		// 	switch (item.name) {
		// 		case '删除':
		// 			this.$refs.dialog.open(close => {
		// 				close();
		// 				console.log('删除文件');
		// 				console.log(this.checkList);
		// 			});
		// 			break;
		// 		default:
		// 			break;
		// 	}
		// }
		handleBottomEvent(item) {
			switch (item.name) {
				case '删除':
					this.$refs.delete.open(close => {
						this.lists = this.lists.filter(item => !item.checked);
						close();
						uni.showToast({
							title: '删除成功',
							icon: 'none'
						});
					});
					break;
				case '重命名':
					this.renameValue = this.checkList[0].name;
					this.$refs.rename.open(close => {
						if (this.renameValue == '') {
							return uni.showToast({
								title: '文件名称不能为空',
								icon: 'none'
							});
						}
						this.checkList[0].name = this.renameValue;
						close();
					});
					break;
				default:
					break;
			}
		},
		openAddDialog() {
			this.$refs.add.open();
		},
		handleAddEvent(item) {
			this.$refs.add.close();
			switch (item.name) {
				case '新建文件夹':
					this.$refs.newdir.open(close => {
						if (this.newdirname == '') {
							return uni.showToast({
								title: '文件夹名称不能为空',
								icon: 'none'
							});
						}
						this.lists.push({
							type: 'dir',
							name: this.newdirname,
							create_time: '2020-10-22 17:00',
							checked: false
						});
						uni.showToast({
							title: '新建文件夹成功',
							icon: 'none'
						});
						close();
					});
					break;
				default:
					break;
			}
		},
		doEvent(item) {
			switch (item.type) {
				case 'image':
					let images = this.lists.filter(item => {
						return item.type === 'image';
					});
					uni.previewImage({
						current: item.data,
						urls: images.map(item => item.data)
					});
					break;
				case 'video':
					uni.navigateTo({
						url: '../video/video?url=' + item.data + '&title=' + item.name
					});
					break;
				default:
					break;
			}
		},
		openSortDialog(){
			this.$refs.sort.open();
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
