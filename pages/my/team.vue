<template>
	<view class="team">
		<header-bar :title="i18n.header.header23"></header-bar>
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
		.bg{
			background-color: $colorC;
		}
	}
}
</style>
