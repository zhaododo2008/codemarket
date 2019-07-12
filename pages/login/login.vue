<template>
	<view>
		<page-head :title="title"></page-head>
		<view class="s-page-wrapper is-100vh">
			<view class="is-33vh has-mgt-10">
				<view class="is-flex is-column is-justify-center  is-align-center is-height-100">
					<image src="../../static/code.png" class="logoimg"></image>
				</view>
			</view>
			<view class="content">
				<view class="has-mglr-10 ">
					<view class=" loginbtn has-radius has-mgtb-20">
						<button @click="tologin('weixin')"> {{ login.loading ? "已登录":"登 录"}} </button>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				login: {
					title: 'login',
					loading: false,
					providerList: [],
					hasLogin: false
				},
				u_token_val:''
			};
		},
		onLoad() {
			uni.getProvider({
				service: 'oauth',
				success: (result) => {
					this.providerList = result.provider.map((value) => {
						let providerName = '';
						switch (value) {
							case 'weixin':
								providerName = '微信登录'
								break;
							case 'qq':
								providerName = 'QQ登录'
								break;
							case 'sinaweibo':
								providerName = '新浪微博登录'
								break;
							case 'xiaomi':
								providerName = '小米登录'
								break;
							case 'alipay':
								providerName = '支付宝登录'
								break;
							case 'baidu':
								providerName = '百度登录'
								break;
							case 'toutiao':
								providerName = '头条登录'
								break;
						}
						return {
							name: providerName,
							id: value
						}
					});

				},
				fail: (error) => {
					console.log('获取登录通道失败', error);
				}
			});

		},
		methods: {
			tologin(provider) {
				const u_token_key = 'u_token_key';
				let u_token_val = this.u_token_val;
				u_token_val = uni.getStorageSync(u_token_key);
				console.log('u_token_val is '+ JSON.stringify(u_token_val))
				
				console.log('to login is ' + provider)
				uni.login({
					provider: provider.id,
					// #ifdef MP-ALIPAY
					scopes: 'auth_user', //支付宝小程序需设置授权类型
					// #endif
					success: (res) => {
						console.log('login success:', res);
						// 更新保存在 store 中的登录状态
				
						this.hasLogin = true;
						this.login.loading = true;
						uni.setStorageSync(u_token_key,res.code);
						
				
					},
					fail: (err) => {
						console.log('login fail:', err);
						this.login.loading = false;
					}
				});


			}
		}
	}
</script>

<style>
	@import url("../../static/css/simplepro.css");

	page {
		min-height: 100%;
		background-color: #FFFFFF;
	}

	.content {
		width: 85%;
		margin: 0 auto;
	}

	.loginbtn button {
		margin-top: 20rpx;
		height: 88rpx;
		width: 100%;
		line-height: 88rpx;
		color: #FFFFFF;
		font-size: 32rpx;
		border-radius: 44rpx;
		outline: 0;
		display: block;
		margin: 0;
		font-family: inherit;
		background: #2F85FC;
		opacity: 0.8;
	}

	button:after {
		border: 2rpx solid #f2f2f2;
	}

	.logoimg {
		width: 200rpx;
		height: 200rpx;
	}
</style>
