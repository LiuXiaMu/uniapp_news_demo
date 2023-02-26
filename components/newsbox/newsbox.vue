<template>
	<view class="news-box">
		<uni-list class="list">
			<uni-list-item :thumb="item.picurl" thumb-size="lg">
				<template v-slot:body>
					<view class="list-item-body">
						<text class="title">{{item.title}}</text>
						<view class="footer" v-if="!item.looktime">
							<text>{{item.author}}</text>
							<text>{{item.hits}} 浏览</text>
						</view>
						<view class="footer" v-else>
							<text>浏览时间：{{item.looktime}}</text>
						</view>
					</view>
				</template>
			</uni-list-item>
		</uni-list>
	</view>
</template>

<script>
	export default {
		props: {
			item: {
				type: Object,
				default () {
					return {
						title: "默认标题",
						author: "小明",
						hits: 666,
						picurl: "https://web-assets.dcloud.net.cn/unidoc/zh/unicloudlogo.png",
					}
				}
			}
		},
		options: {
			styleIsolation: 'shared'
		},
		data() {
			return {}
		}
	}
</script>

<style>
	@import "newsbox.css";
</style>
