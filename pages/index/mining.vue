<template>
	<view class="mining">
		<header-bar :title="i18n.header.header9"></header-bar>
		<view class="container">
			<view class="form-wrap">
				<view class="form-wrap-top">
					<view class="left">{{i18n.exchange.lang23}}</view>
					<view class="right">{{indexData.acc?indexData.acc.QUSD:'0.0000'}}</view>
				</view>
				<view class="form-wrap-bot">
					<view class="form-item">
						<input type="digit" v-model="current.amount" :placeholder="i18n.exchange.lang30" />
					</view>
					<view class="form-item">
						<input type="text" v-model="current.paypass" :password="active" :placeholder="i18n.exchange.lang26" />
						<view class="password" :class="{'active': !active}" @tap="addClass('active')"></view>
					</view>
				</view>
			</view>
			<view class="form-submit-wrap">
				<view class="form-submit gradient-blue" @tap="submit">{{i18n.exchange.lang31}}</view>
			</view>
		</view>
	</view>
</template>

<script>
	import HeaderBar from '../../components/header-bar.vue';
	export default {
		data() {
			return {
				active: true,
				current: {
					amount: '',
					paypass: '',
				},
				indexData: '',        //QUSD数量
			}
		},
		components:{
			HeaderBar
		},
		onLoad() {
			this.getData()
		},
		methods: {
			addClass(classname){
				this[classname] = !this[classname]
			},
			submit(){
				// 提交信息
				let { current } = this
				this.uniRequest({
					url: 'mining',
					data: {
						...current
					}
				}).then(res => {
					uni.showToast({
						title: res.message,
						icon: 'none',
						success: () => {
							this.getData()
							this.current = {
								amount: '',
								paypass: '',
							}
							setTimeout(() => {
								uni.reLaunch({
									url: '/pages/index/index'
								})
							}, 500)
						}
					})
				})
			},
			getData(){
				// 获取USDT
				this.uniRequest({
					url: 'wallet',
					method: 'GET'
				}).then(res => {
					this.indexData = res.result
				})
			}
		},
		computed: {  
			i18n () {  
				return this.$t('index')  
			}  
		}
	}
</script>

<style lang="scss">
.mining{
	
}
</style>
