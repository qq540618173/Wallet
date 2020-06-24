<template>
	<view class="team">
		<header-bar :title="i18n.header.header23"></header-bar>
		<view class="container">
			<view class="team-wrap" v-for="(item, index) in teamList" :key="index">
				<view class="team-top">
					<view class="team-top-left">
						<view class="phone">
							<text>{{item.phone}}({{item.level}}{{i18n.my.lang71}})</text>
						</view>
						<view class="time">
							<text>{{i18n.my.lang67}}:{{item.create_time}}</text>
						</view>
					</view>
					<view class="team-top-right">
						<view class="amount">
							<text>{{i18n.my.lang66}}</text>
						</view>
						<view class="money">
							<text>{{item.invest}}QUSD</text>
						</view>
					</view>
				</view>
				<view class="team-bot">
					<view class="team-bot-item">
						<view class="left">
							<text>{{i18n.my.lang68}}</text>
						</view>
						<view class="right">
							<text>{{item.total}}QUSD</text>
						</view>
					</view>
					<view class="team-bot-item">
						<view class="left">
							<text>{{i18n.my.lang69}}</text>
						</view>
						<view class="right">
							<text>{{item.teams}}{{i18n.my.lang70}}</text>
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import HeaderBar from '../../components/header-bar.vue';
	export default {
		data() {
			return {
				teamList: [],
			}
		},
		components:{
			HeaderBar
		},
		onLoad() {
			this.getData()
		},
		methods: {
			getData(){
				this.uniRequest({
					url: 'teams',
					method: 'GET'
				}).then(res => {
					this.teamList = res.result
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
	.container{
		padding: 20rpx 0;
		.team-wrap{
			background-color: $colorC;
			padding: 30rpx 30rpx 42rpx;
			position: relative;
			&::after{
				content: "";
				display: block;
				height: 2rpx;
				background: url('../../static/images/icon19.png') repeat-x;
				position: absolute;
				left: 30rpx;
				right: 30rpx;
				bottom: 0;
			}
			&:last-child::after{
				display: none;
			}
			.team-top{
				display: flex;
				justify-content: space-between;
				align-items: center;
				margin-bottom: 36rpx;
				.team-top-left{
					.phone{
						font-size: $fontI;
						color: $colorA;
						line-height: 40rpx;
					}
					.time{
						font-size: $fontk;
						color: $colorB;
						line-height: 28rpx;
						margin-top: 6rpx;
					}
				}
				.team-top-right{
					text-align: right;
					.amount{
						font-size: $fontJ;
						color: $colorB;
						line-height: 28rpx;
					}
					.money{
						font-size: $fontI;
						color: $colorF;
						line-height: 40rpx;
						margin-top: 6rpx;
					}
				}
			}
			.team-bot{
				padding: 28rpx 30rpx;
				background-color: #292F42;
				border-radius: 8rpx;
				.team-bot-item{
					display: flex;
					justify-content: space-between;
					align-items: center;
					margin-bottom: 24rpx;
					&:last-child{
						margin-bottom: 0;
					}
					.left{
						font-size: $fontJ;
						color: $colorB;
					}
					.right{
						font-size: $fontJ;
						color: $colorA;
					}
				}
			}
		}
	}
}
</style>
