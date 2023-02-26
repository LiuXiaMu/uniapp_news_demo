<template>
	<view class="home">
		<scroll-view scroll-x class="nav-scroll">
			<view class="uni-padding-wrap uni-common-mt">
				<uni-segmented-control :current="current" :values="items" style-type="text" :active-color="activeColor"
					@clickItem="onClickItem" />
			</view>
		</scroll-view>
		<view class="content">
			<view v-for="(item,index) in datas" :key="item.id">
				<newsbox :item="item" @click.native="goDetail(item)"></newsbox>
			</view>
			<view class="no-data" v-if="!datas.length && this.loading==2">
				<image src="../../static/images/noData-3.png"></image>
			</view>
		</view>

		<view class="loading" v-if="datas.length">
			<view></view>
			<view v-show="loading==1">数据加载中。。。</view>
			<view v-show="loading==2">没有更多了~~</view>
		</view>
	</view>
</template>

<script>
	export default {
		options: {
			styleIsolation: 'shared'
		},
		data() {
			return {
				items: [], //导航栏名字
				itemsId: [], //导航栏ID
				datas: [], //列表数据
				current: 0,
				activeColor: '#31C280',
				page: 1, //分页
				nums: 10, //一页6条数据
				loading: 0, //0 默认 1加载中 2没有更多了
			}
		},
		async created() {
			await this.getNavData()
			await this.getListData(0)
		},
		//上拉刷新
		onReachBottom() {
			if (this.loading == 2) {
				return;
			}
			this.page++
			this.loading = 1
			this.getListData(this.current)
		},
		methods: {
			onClickItem(e) {
				this.page = 1
				this.datas = []
				this.loading = 0
				this.getListData(e.currentIndex)
				this.current = e.currentIndex
			},
			//跳转到详情页
			goDetail(item) {
				uni.navigateTo({
					url: `/pages/detail/index?cid=${item.classid}&id=${item.id}`
				})
			},
			//获取导航栏数据
			getNavData() {
				return new Promise((resolve, reject) => {
					uni.request({
						url: "https://ku.qingnian8.com/dataApi/news/navlist.php",
						success: res => {
							let data = res.data
							data.map(item => {
								this.items.push(item.classname)
								this.itemsId.push(item.id)
							})
							resolve()
						},
						fail: err => {
							reject()
						}
					})
				})

			},
			//获取列表数据
			getListData(value) {
				return new Promise((resolve, reject) => {
					uni.request({
						url: "https://ku.qingnian8.com/dataApi/news/newslist.php",
						data: {
							cid: this.itemsId[value],
							page: this.page,
							num: this.nums
						},
						success: res => {
							if (res.data.length === 0) {
								this.loading = 2
							}
							this.datas = [...this.datas, ...res.data]
							resolve()
						},
						fail: err => {
							reject()
						}
					})
				})

			}

		}
	}
</script>

<style lang="less" scoped>
	@import './index.css';
</style>
