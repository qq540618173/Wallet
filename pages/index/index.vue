<template>
	<view class="index">
		<header-bar :isBack="isBack" :isBg="isBg" :isSlot="isSlot" :title="i18n.header.header1">
			<text slot="text" @tap="gotoPage('history')">{{i18n.header.header26}}</text>
		</header-bar>
		<view class="content">
			<view class="header-data" v-if="indexData.acc">
				<view v-for="(item, index) in indexData.acc" :key="index">{{index}}: {{item}}<text></text></view>
			</view>
			<view class="operation-wrap">
				<view class="operation-item gradient1" @tap="gotoPage('recharge')">
					<image src="../../static/images/icon5.png"></image>
					<text>{{i18n.index.lang1}}</text>
				</view>
				<view class="operation-item gradient2" @tap="gotoPage('withdrawal')">
					<image src="../../static/images/icon6.png"></image>
					<text>{{i18n.index.lang2}}</text>
				</view>
				<view class="operation-item gradient3" @tap="gotoPage('exchange')">
					<image src="../../static/images/icon7.png"></image>
					<text>{{i18n.index.lang3}}</text>
				</view>
			</view>
			<view class="notice-wrap">
				<view class="qrcode" @tap="gotoPage('invitation')">
					<view class="qrbox"><image src="../../static/images/icon10.png"></image></view>
					<text>{{i18n.index.lang4}}</text>
				</view>
				<view class="notice">
					<view class="notice-text">
						<text>{{i18n.index.lang4_1}}</text>
					</view>
					<view class="notice-box">
						<view class="uni-padding-wrap">
							<view class="page-section swiper">
								<view class="page-section-spacing">
									<swiper class="swiper" autoplay="autoplay" interval="3000" duration="500" circular="circular" vertical="vertical">
										<swiper-item v-for="(item, index) in noticeList" :key="index">
											<view class="swiper-item">
												<view class="notice-item" @tap="gotoPage(`/pages/my/newsdetail?id=${item.id}`)">{{item.title}}</view>
											</view>
										</swiper-item>
									</swiper>
								</view>
							</view>
						</view>
					</view>
					<view class="more" @tap="gotoPage('/pages/my/newslist')">
						<text>{{i18n.index.lang8_2}}</text>
					</view>
				</view>
			</view>
			<view class="mine">
				<view :class="i18n.index.lang5"></view>
				<view class="mine-wrap">
					<view class="mine-item gradient-blue" @tap="gotoPage('mining')">
						<image src="../../static/images/icon11.png"></image>
						<text>{{i18n.index.lang6}}</text>
					</view>
					<view class="mine-item gradient-green" @tap="showModal">
						<image src="../../static/images/icon12.png"></image>
						<text>{{i18n.index.lang7}}</text>
					</view>
				</view>
				<view class="tips">
					<text>{{i18n.index.lang8}}：{{indexData.sub_pool}}</text>
				</view>
				<view class="amount">
					<view class="top">{{i18n.index.lang9}}</view>
					<view class="mid">{{indexData.pool?indexData.pool.amount:0}}</view>
					<view class="mid2">
						<text>{{i18n.index.lang8_1}}：{{indexData.queue_pool}}</text>
					</view>
					<view class="bot">
						<image src="../../static/images/icon13.png"></image>
						<text>{{i18n.index.lang10}}：{{indexData.pool?indexData.pool.day:0}} {{i18n.index.lang11}}</text>
					</view>
				</view>
			</view>
		</view>
		<tabbar :isCurrent="1"></tabbar>
		<modal :modalActive="modalActive">
			<view slot="modal-content">
				<view class="title">
					<text>{{i18n.index.lang7}}</text>
				</view>
				<view class="form-item">
					<input type="text" v-model="payPass" :password="active" :placeholder="i18n.my.lang73" />
					<view class="password" :class="{'active': !active}" @tap="addClass('active')"></view>
				</view>
				<view class="modal-tips">
					<text>{{i18n.index.lang15_1}}</text>
				</view>
				<view class="operation-wrap">
					<view class="operation-btn gradient-green" @tap="cancel">
						<text>{{i18n.index.lang15_2}}</text>
					</view>
					<view class="operation-btn gradient-blue" @tap="stopMine">
						<text>{{i18n.index.lang15_3}}</text>
					</view>
				</view>
			</view>
		</modal>
	</view>
</template>

<script>
	import HeaderBar from '../../components/header-bar.vue'
	import Tabbar from '../../components/tabbar.vue'
	import Modal from '../../components/modal.vue'
	export default {
		data() {
			return {
				title: '',
				timer: '',
				isBack: false,
				isBg: true,
				isSlot: true,
				indexData: {},  //总数据
				noticeList: [], //公告列表
				modalActive: false,
				active: true,
				payPass: '',
			}
		},
		components: {
			HeaderBar,
			Tabbar,
			Modal
		},
		onLoad() {
			this.getData()
			this.getNotice()
		},
		onShow() {
			this.getData()
		},
		methods: {
			getData(){
				// 获取首页信息
				this.uniRequest({
					url: 'wallet',
					method: 'GET'
				}).then(res => {
					this.indexData = res.result
				})
			},
			getNotice(){
				// 获取首页公告
				this.uniRequest({
					url: 'notice',
					data: {
						page: 1,
						limit: 5
					}
				}).then(res => {
					this.noticeList = res.result.data
				})
			},
			showModal(){
				this.modalActive = true
			},
			cancel(){
				this.payPass = ''
				this.modalActive = false
			},
			stopMine(){
				// 停止挖矿
				let { payPass } = this
				this.uniRequest({
					url: 'stopMine',
					data: {
						payPass
					}
				}).then(res => {
					uni.showToast({
						title: '已停止挖矿',
						icon: 'none',
						success: res => {
							this.getData()
							this.cancel()
						}
					})
				})
			},
			addClass(classname){
				this[classname] = !this[classname]
			},
			gotoPage(url){
				uni.navigateTo({
				    url
				});
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
/* #ifdef H5 */
uni-page-body{
	padding-bottom: 100rpx;
}
/* #endif */
.index{
	min-height: 100%;
	.content {
		width: 100%;
		min-height: 100%;
		box-sizing: border-box;
		padding: 0 30rpx 30rpx;
		background: #292F42 url('../../static/images/bot.png') center top no-repeat;
		background-size: contain;
		.swiper1{
			margin-top: 40rpx;
			image{
				width: 100%;
				height: 100%;
			}
		}
		.header-data{
			font-size: 48rpx;
			color: #FFFFFF;
			view{
				margin-bottom: 20rpx;
				text{
					font-size: 40rpx;
				}
			}
		}
		.operation-wrap{
			display: flex;
			justify-content: space-between;
			padding-top: 40rpx;
			.operation-item{
				flex: 1;
				display: flex;
				justify-content: center;
				align-items: center;
				width: 216rpx;
				height: 120rpx;
				border-radius: 8rpx;
				background: #FFFFFF;
				word-wrap: break-word;
				image{
					width: 56rpx;
					height: 56rpx;
				}
				text{
					font-size: 32rpx;
					color: #FFFFFF;
					margin-left: 16rpx;
				}
			}
			.gradient1{
				background:linear-gradient(135deg,rgba(4,190,217,1) 0%,rgba(98,34,225,1) 100%);
			}
			.gradient2{
				background:linear-gradient(135deg,rgba(135,189,250,1) 0%,rgba(140,47,238,1) 100%);
				margin: 0 20rpx;
			}
			.gradient3{
				background:linear-gradient(135deg,rgba(115,202,213,1) 0%,rgba(37,164,141,1) 100%);
			}
		}
		.notice-wrap{
			height: 140rpx;
			border-radius: 8rpx;
			background-color: #222636;
			box-sizing: border-box;
			padding: 20rpx;
			margin-top: 30rpx;
			display: flex;
			justify-content: space-between;
			.qrcode{
				width: 212rpx;
				height: 96rpx;
				display: flex;
				align-items: center;
				justify-content: center;
				border-radius: 8rpx;
				background-color: #292F42;
				.qrbox{
					width: 60rpx;
					height: 60rpx;
					margin-right: 12rpx;
					image{
						width: 60rpx;
						height: 60rpx;
					}
				}
				text{
					color: #C4C4C9;
					font-size: 32rpx;
				}
			}
		}
		.notice{
			width: 400rpx;
			padding-left: 64rpx;
			box-sizing: border-box;
			background: url('../../static/images/icon8.png') 18rpx no-repeat;
			background-size: 40rpx 40rpx;
			position: relative;
			display: flex;
			align-items: center;
			&::after{
				content: "";
				display: block;
				width: 1rpx;
				height: 56rpx;
				background-color: #292F42;
				position: absolute;
				left: 0;
				top: 50%;
				transform: translateY(-50%);
			}
			.notice-text{
				font-size: $fontJ;
				color: $colorB;
				padding-right: 20rpx;
			}
		}
		.notice-box{
			height: 68rpx;
			overflow: hidden;
			flex: 1;
			uni-swiper{
				height: 68rpx;
				.notice-item{
					height: 68rpx;
					line-height: 68rpx;
					overflow: hidden;
					text-overflow: ellipsis;
					white-space: nowrap;
					font-size: $fontJ;
					color: $colorB;
				}
			}
		}
		.more{
			font-size: $fontJ;
			color: $colorF;
			padding-left: 10rpx;
		}
		.mine{
			border-radius: 8rpx;
			background-color: #222636;
			padding: 44rpx 40rpx 26rpx;
			.title{
				width: 226rpx;
				height: 38rpx;
				background: url('../../static/images/icon9.png') center no-repeat;
				background-size: contain;
				margin: 0 auto;
			}
			.subtitle{
				width: 292rpx;
				height: 36rpx;
				background: url('../../static/images/icon9_1.png') center no-repeat;
				background-size: contain;
				margin: 0 auto;
			}
			.mine-wrap{
				display: flex;
				justify-content: space-between;
				margin-top: 42rpx;
				.mine-item{
					width: 260rpx;
					height: 96rpx;
					border-radius: 8rpx;
					display: flex;
					align-items: center;
					justify-content: center;
					image{
						width: 48rpx;
						height: 48rpx;
					}
					text{
						font-size: $fontH;
						color: $colorA;
						margin-left: 14rpx;
					}
				}
			}
			.tips{
				margin-top: 32rpx;
				background-color: #1C1F2A;
				border-radius: 8rpx;
				line-height: 100rpx;
				color: #999999;
				font-size: $fontH;
				padding: 0 30rpx;
				display: flex;
				justify-content: space-between;
				align-items: center;
			}
			.amount{
				margin-top: 22rpx;
				background-color: #1C1F2A;
				border-radius: 8rpx;
				text-align: center;
				padding: 40rpx 0;
				.top{
					color: $colorB;
					font-size: $fontJ;
				}
				.mid{
					color: $colorA;
					font-size: $fontZ;
					margin-top: 8rpx;
				}
				.mid2{
					font-size: $fontJ;
					color: $colorE;
					margin: 14rpx 30rpx 0;
					padding: 20rpx 0;
					border-radius: 8rpx;
					background-color: #13151B;
				}
				.bot{
					margin-top: 40rpx;
					color: $colorF;
					font-size: $fontJ;
					display: flex;
					align-items: center;
					justify-content: center;
					image{
						width: 30rpx;
						height: 30rpx;
						margin-right: 12rpx;
					}
				}
			}
		}
	}
	.modal{
		.title{
			text-align: center;
			font-size: $fontE;
			color: $colorG;
			margin-bottom: 42rpx;
		}
		.modal-tips{
			font-size: $fontI;
			color: $colorA;
			margin-top: 28rpx;
		}
		.operation-wrap{
			display: flex;
			justify-content: space-between;
			align-items: center;
			.operation-btn{
				height: 104rpx;
				line-height: 104rpx;
				border-radius: 8rpx;
				text-align: center;
				font-size: $fontG;
				color: $colorA;
				margin-top: 100rpx;
				flex: 1;
				&:first-child{
					margin-right: 20rpx;
				}
			}
		}
	}
}
</style>
