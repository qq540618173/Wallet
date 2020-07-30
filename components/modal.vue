<template>
	<view>
		<view class="mask" v-if="modalActive"></view>
		<view class="modal" :class="{'active': modalActive}">
			<slot name="modal-content"></slot>
		</view>
	</view>
</template>

<script>
	export default {
		props: {
			modalActive: {
				type: Boolean,
				default: false
			}
		}
	}
</script>

<style lang="scss">
.mask{
	position: fixed;
	top: 0;
	left: 0;
	right: 0;
	bottom: 0;
	background: rgba(0,0,0, .4);
	z-index: 998;
}
.modal{
	width: 100vw;
	padding: 42rpx 30rpx 48rpx;
	border-radius: 20rpx 20rpx 0rpx 0rpx;
	background-color: $colorC;
	position: fixed;
	bottom: 100rpx;
	z-index: 999;
	box-shadow: 0 0 20rpx #000000;
	box-sizing: border-box;
	transform: translateY(200%);
	transition: all 0.5s;
	&.active{
		display: block;
		transform: translateY(0);
		transition: all 0.5s;
	}
}
</style>
