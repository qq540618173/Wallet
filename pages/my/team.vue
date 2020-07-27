<template>
	<view class="team">
		<header-bar :title="i18n.header.header23"></header-bar>
		<view class="search-box">
			<view class="search">
				<input type="text" v-model="phone" :placeholder="i18n.login.lang84" />
				<view class="btn" @tap="getData"></view>
			</view>
		</view>
		<view class="container">
			<view class="bg">
				<Treelist :dataList="teamList"></Treelist>
			</view>
		</view>
	</view>
</template>

<script>
	import HeaderBar from '../../components/header-bar.vue';
	import Treelist from '../../components/treelist.vue';
	export default {
		data() {
			return {
				teamList: [],
				phone: '',
			}
		},
		components:{
			HeaderBar,
			Treelist
		},
		onLoad() {
			this.getData()
		},
		methods: {
			getData(){
				let { phone } = this
				this.uniRequest({
					url: 'teams',
					method: 'GET',
					data: {
						phone
					}
				}).then(res => {
					this.teamList = res.result
					this.phone = ''
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
.team{
	.search-box{
		background-color: $colorC;
		padding: 20rpx 30rpx 32rpx;
		.search{
			display: flex;
			justify-content: space-between;
			align-items: center;
			height: 72rpx;
			border-bottom: 2rpx solid $colorF;
			input{
				flex: 1;
				height: 72rpx;
				border: none;
				background: none;
				font-size: $fontH;
				color: $colorA;
			}
			.btn{
				width: 72rpx;
				height: 72rpx;
				background: url('../../static/images/icon50.png') center no-repeat;
				background-size: 40rpx 40rpx;
			}
		}
	}
	.container{
		padding: 20rpx 0;
		.bg{
			background-color: $colorC;
		}
	}
}
</style>
