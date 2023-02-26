<template>
	<view class="user">
		<view class="top">
			<image src="../../static/images/history.png"></image>
			<p>浏览历史</p>
		</view>
		<view class="content">
			<uni-swipe-action>
				<uni-swipe-action-item class="content-box" v-for="(item,index) in datas" :key="item.id">
					<template v-slot:right>
						<view class="slot-button" @click="bindClick(item)">
							删除
						</view>
					</template>
					<newsbox :item="item" @click.native="goDetail(item)"></newsbox>
				</uni-swipe-action-item>
			</uni-swipe-action>
			<view class="info" v-if="!datas.length">
				<image src="../../static/images/noData-4.png"></image>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				datas: [],

			}
		},
		onShow() {
			this.getData()
		},
		methods: {
			bindClick(item) {
				let index = this.datas.findIndex(i => {
					return item.id == i.id
				})
				this.$nextTick(() => {
					this.datas.splice(index, 1)
					uni.setStorageSync('historyArr', this.datas)
				})
			},
			//跳转到详情页
			goDetail(item) {
				uni.navigateTo({
					url: `/pages/detail/index?cid=${item.classid}&id=${item.id}`
				})
			},
			//从存储里拿数据
			getData() {
				this.datas = uni.getStorageSync('historyArr')
			},
		}
	}
</script>

<style scoped>
	@import "index.css";
</style>
