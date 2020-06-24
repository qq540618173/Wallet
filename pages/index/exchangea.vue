<template>
	<view class="exchangea">
		<header-bar :title="i18n.header.header7"></header-bar>
		<view class="container">
			<view class="form-wrap">
				<view class="form-wrap-top">
					<view class="left">{{i18n.exchange.lang28}}</view>
					<view class="right">{{indexData.acc?indexData.acc.USDT:'0.0000'}}</view>
				</view>
				<view class="form-wrap-bot">
					<view class="form-item">
						<input type="number" v-model="current.from_amount" :placeholder="i18n.exchange.lang24" />
					</view>
					<view class="form-item">
						<label>{{i18n.exchange.lang29}}:</label>
						<input class="shrink" type="number" v-model="current.from_amount?calc:''" placeholder="0.00" disabled="true" />
					</view>
					<view class="form-item">
						<input type="text" v-model="current.paypass" :password="active" :placeholder="i18n.exchange.lang26" />
						<view class="password" :class="{'active': !active}" @tap="addClass('active')"></view>
					</view>
				</view>
			</view>
			<view class="form-submit-wrap">
				<view class="form-submit gradient-blue" @tap="submit">{{i18n.exchange.lang27}}</view>
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
					from_amount: '',
					paypass: '',
					from_coin: 'USDT',
					to_coin: 'QUSD',
				},
				rate: '',             //汇率
				indexData: '',        //USDT数量
			}
		},
		onLoad() {
			this.getData()
		},
		components:{
			HeaderBar
		},
		methods: {
			submit(){
				// 提交信息
				let { current } = this
				this.uniRequest({
					url: 'exchange',
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
								from_amount: '',
								paypass: '',
								from_coin: 'USDT',
								to_coin: 'QUSD',
							}
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
			},
			addClass(classname){
				this[classname] = !this[classname]
			}
		},
		computed: {  
			i18n () {  
				return this.$t('index')  
			},
			calc(){
				return this.current.from_amount
			}
		}
	}
</script>

<style lang="scss">
.exchangea{
	.shrink{
		min-width: 200rpx;
	}
}
</style>
