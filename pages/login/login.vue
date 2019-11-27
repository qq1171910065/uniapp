<template>
	<view>
		<block v-for="(item,index) in providerList" :key="index">
			<view @tap="tologin(item)">{{item.name}}</view>
		</block>
	</view>
</template>
<script>
	// import {
	// 	mapState,
	// 	mapMutations
	// } from 'vuex'

	export default {
		data() {
			return {
				providerList: []
			}
		},
		computed: {

		},
		onLoad() {
			this.getLoginAuth()
		},
		methods: {
			//获取当前登录信息
			getLoginAuth() {
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
									providerName = '新浪微博';
							}
							return { 
								name: providerName,
								id: value
							}
						});
						console.log(JSON.stringify(this.providerList))
					},
					fail: (error) => {
						console.log('获取登录通道失败', error);
					}
				});
			},
			//登录
			tologin(provider) {
				uni.login({
					provider: provider.id,
					success: (res) => {
						console.log('login success:', res);
						// 更新保存在 store 中的登录状态
					},
					fail: (err) => {
						console.log('login fail:', err);
					}
				});
			}
		}
	}
</script>

<style>
	button {
		background-color: #007aff;
		color: #ffffff;
	}
</style>
