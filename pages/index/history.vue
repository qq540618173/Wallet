<template>
	<view class="history">
		<header-bar :title="i18n.header.header26"></header-bar>
		<view class="container">
			<scroll-view class="scrollview" scroll-y="true" lower-threshold="100" @scrolltolower="loadMore" v-if="historyList.length">
				<view class="history-wrap" v-for="(item, index) in historyList" :key="index">
					<view class="datetime">{{item.create_time}}</view>
					<view class="history-box">
						<view class="top">
							<text v-if="item.is_me == 0">{{i18n.history['history'+item.type]}}</text>
							<text v-if="item.is_me == 1">{{i18n.history.history3 + item.remark}}</text>
							<text v-if="item.is_me == 2">{{i18n.history.history3_1 + item.remark + i18n.history.history3_2}}</text>
							<text>{{i18n.history.history14}}</text>
						</view>
						<view class="bot">
							<text :class="item.state>0?'minus':'plus'">{{item.state>0?'-':'+'}}{{item.amount + item.coin}}</text>
							<text>{{item.balance+item.coin}}</text>
						</view>
					</view>
				</view>
			</scroll-view>
			<view class="nodata" v-if="!historyList.length">
				<text>{{i18n.history.history15}}</text>
			</view>
		</view>
	</view>
</template>

<script>
	import HeaderBar from '../../components/header-bar.vue';
	export default {
		data() {
			return {
				pageNum: 1,
				total: '',
				historyList: [],
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
				let { pageNum } = this
				this.uniRequest({
					url: 'accountLogs',
					method: 'GET',
					data: {
						page: pageNum,
						limit: 10
					}
				}).then(res => {
					this.total = res.result.total
					this.historyList = this.historyList.concat(res.result.data)
				})
			},
			loadMore(){
				// 加载更多
				if(this.historyList.length < this.total){
					this.pageNum+=1
					this.getData()
				}else{
					uni.showToast({
						title: '没有更多了',
						icon: 'none'
					})
				}
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
.history{
	.container{
		.scrollview{
			height: calc(100vh - 188rpx);
		}
		.history-wrap{
			background-color: $colorC;
			border-radius: 8rpx;
			padding: 28rpx;
			margin-bottom: 20rpx;
			&:last-child{
				margin-bottom: 0;
			}
			.datetime{
				font-size: $fontJ;
				color: $colorA;
			}
			.history-box{
				margin-top: 20rpx;
				.top{
					font-size: $fontJ;
					color: $colorA;
					margin-bottom: 6rpx;
					display: flex;
					justify-content: space-between;
					align-items: center;
				}
				.bot{
					font-size: $fontI;
					display: flex;
					justify-content: space-between;
					align-items: center;
					color: $colorA;
					.plus{
						color: #DB4D43;
					}
					.minus{
						color: #33D38A;
					}
				}
			}
		}
		.nodata{
			font-size: $fontG;
			color: $colorA;
			text-align: center;
			line-height: 300rpx;
		}
	}
}
</style>
