<template>
	<view class="detail">
		<view class="title">
			<text>{{formData.title}}</text>
		</view>
		<view class="info">
			<text>编辑：{{formData.author}}</text>
			<text>发布时间：{{formData.posttime}}</text>
		</view>
		<view class="content">
			<rich-text :nodes="formData.content"></rich-text>
		</view>
		<view class="prompt-msg" v-if="formData.content||isShow">
			<uni-notice-bar backgroundColor="#FDEFF0" color="#E9B6B7"
				text="声明：本站的内容均采集与腾讯新闻，如果侵权请联系管理(513894357@qq.com)进行整改删除,本站进行了内容采集不代表本站及作者观点，若有侵犯请及时联系管理员，谢谢您的支持。" />
		</view>
	</view>
</template>

<script>
	import {
		dateFormat
	} from "../../utils/util.js"
	export default {
		onLoad(options) { //此处接收传递过来的参数wx.navigateTo跳转时传递的参数
			uni.request({
				url: "https://ku.qingnian8.com/dataApi/news/detail.php",
				data: options,
				success: res => {
					let data = Object.assign({}, res.data)
					data.content = data.content.replace(/<img/gi, '<img style="max-width:100%;"')
					data.posttime = dateFormat(data.posttime)
					this.formData = data
					if (data.content == "") {
						this.isShow = true
					}
					uni.setNavigationBarTitle({
						title: this.formData.title
					})
					this.saveHistory()
				}
			})
			//如果要在页面中使用
		},
		data() {
			return {
				formData: {},
				isShow: false
			}
		},
		methods: {
			saveHistory() {
				let historyArr = uni.getStorageSync("historyArr") || []
				let form = {
					id: this.formData.id,
					classid: this.formData.classid,
					picurl: this.formData.picurl,
					title: this.formData.title,
					looktime: dateFormat(Date.now() / 1000)
				}
				let index = historyArr.findIndex(item => {
					return item.id == form.id
				})
				if (index >= 0) {
					historyArr.splice(index, 1)
				} else if (historyArr.length >= 10) {
					historyArr.pop()
				}
				historyArr.unshift(form)
				uni.setStorageSync("historyArr", historyArr)
			},
		}
	}
</script>

<style>
	@import "index.css";
</style>
