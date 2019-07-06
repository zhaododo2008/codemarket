<template>
	<view>
		<view class="banner">
			<image class="banner-img" :src="banner.image_url"></image>
			<view class="banner-title">{{banner.title}}</view>
		</view>
		<view class="article-meta">
			<text class="article-author">{{banner.source}}</text>
			<text class="article-text">发表于</text>
			<text class="article-time">{{banner.datetime}}</text>
		</view>
		<view class="article-content">
			<view class="uni-padding-wrap">
				<uParse :content="parentContent" @preview="preview" @navigate="navigate" />
			</view>
		</view>
		<view class="comment-wrap"></view>
	</view>
</template>

<script>
	import uParse from '../../components/uParse/src/wxParse.vue'
	const FAIL_CONTENT = '<p>获取信息失败</p>';
	export default {
		components: {
			uParse
		},
		data() {
			return {
				banner: {},
				parentContent:''
			}
		},
		onShareAppMessage() {
			return {
				title: this.banner.title,
				path: '/pages/detail/detail?query=' + JSON.stringify(this.banner)
			}
		},
		onLoad(event) {
			console.log('onLoad event start')
			// 目前在某些平台参数会被主动 decode，暂时这样处理。
			try {
				this.banner = JSON.parse(decodeURIComponent(event.query));
				
				const cpBanner = Object.assign({},this.banner)
				this.parentContent = cpBanner.detail
			} catch (error) {
				this.banner = JSON.parse(event.query);
			}
			uni.setNavigationBarTitle({
				title: this.banner.title
			});
		},
		methods: {

			preview(src, e) {
				// do something
				console.log("src: " + src);
			},
			navigate(href, e) {

				const url = '/pages/navigator/new-page/new-page' + '?data=' + href

				uni.navigateTo({
					url: url,
				})

			}
		}
	}
</script>

<style>
	@import url("../../components/uParse/src/wxParse.css");

	.banner {
		height: 360upx;
		overflow: hidden;
		position: relative;
		background-color: #ccc;
	}

	.banner-img {
		width: 100%;
	}

	.banner-title {
		max-height: 84upx;
		overflow: hidden;
		position: absolute;
		left: 30upx;
		bottom: 30upx;
		width: 90%;
		font-size: 32upx;
		font-weight: 400;
		line-height: 42upx;
		color: white;
		z-index: 11;
	}

	.article-meta {
		padding: 20upx 40upx;
		display: flex;
		flex-direction: row;
		justify-content: flex-start;
		color: gray;
	}

	.article-text {
		font-size: 26upx;
		line-height: 50upx;
		margin: 0 20upx;
	}

	.article-author,
	.article-time {
		font-size: 30upx;
	}

	.article-content {
		padding: 0 30upx;
		overflow: hidden;
		font-size: 30upx;
		margin-bottom: 30upx;
	}
</style>
