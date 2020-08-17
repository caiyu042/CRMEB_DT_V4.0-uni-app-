<template>
	<view class='index-wrapper' v-if="fastList.length" :class="{borderShow:isBorader}">
		<view class='title acea-row row-between-wrapper'>
			<view class='text'>
				<view class='name line1'>{{titleInfo[0].val}}</view>
				<view class='line1'>{{titleInfo[1].val}}</view>
			</view>
			<view class='more' @click="gopage('/pages/goods_cate/goods_cate')">
				更多
				<text class='iconfont icon-jiantou'></text>
			</view>
		</view>
		<view class='scroll-product'>
			<scroll-view class="scroll-view_x" scroll-x style="width:auto;overflow:hidden;">
				<block v-for="(item,index) in fastList" :key='index'>
					<view hover-class="none" class='item' @click="gopage('/pages/goods_cate/goods_cate?sid='+item.id+'&title='+item.cate_name)">
						<view class='img-box'>
							<image :src='item.pic'></image>
						</view>
						<view class='pro-info line1'>{{item.cate_name}}</view>
					</view>
				</block>
			</scroll-view>
		</view>
	</view>
</template>

<script>
	let app = getApp()
	import {
		mapState
	} from 'vuex'
	import {
		goPage
	} from '@/libs/order.js'
	export default {
		name: 'f_scroll_box',
		props: {
			dataConfig: {
				type: Object,
				default: () => {}
			},
			activeName: {
				type: String,
				default: ''
			}
		},
		computed: {
			...mapState({
				data: state => state.indexData.indexDatas.info
			})
		},
		watch: {
			'data': {
				handler(nVal, oVal) {
					this.fastList = nVal.fastList
				},
				deep: true
			},
			activeName: {
				handler(nVal, oVal) {
					if (nVal == this.name && app.globalData.isIframe) {
						this.isBorader = true
					} else {
						this.isBorader = false
					}
				},
				deep: true
			}
		},
		data() {
			return {
				titleInfo: this.dataConfig.titleInfo.list[0].chiild,
				fastInfo: '上百种商品分类任您选择',
				fastList: [],
				isBorader:false,
				name: this.$options.name
			}
		},
		mounted() {


		},
		methods: {
			// 
			gopage(url) {
				goPage().then(res => {
					uni.switchTab({
						url: url,
					});

				})
			}
		}
	}
</script>

<style lang="scss">
	.scroll-product {
		white-space: nowrap;
		margin-top: 38rpx;
		padding: 0 30rpx 37rpx 30rpx;
	}

	.scroll-product .item {
		width: 180rpx;
		display: inline-block;
		margin-right: 19rpx;
		border-bottom: 4rpx solid #47b479;
		box-shadow: 0 40rpx 30rpx -10rpx #eee;
	}

	.scroll-product .item:nth-of-type(3n) {
		border-bottom: 4rpx solid #ff6960;
	}

	.scroll-product .item:nth-of-type(3n-1) {
		border-bottom: 4rpx solid #579afe;
	}

	.scroll-product .item:nth-last-child(1) {
		margin-right: 0;
	}

	.scroll-product .item .img-box {
		width: 100%;
		height: 180rpx;
	}

	.scroll-product .item .img-box image {
		width: 100%;
		height: 100%;
		border-radius: 6rpx 6rpx 0 0;
	}

	.scroll-product .item .pro-info {
		font-size: 24rpx;
		color: #282828;
		text-align: center;
		height: 60rpx;
		line-height: 60rpx;
		border: 1rpx solid #f5f5f5;
		border-bottom: 0;
		border-top: 0;
		padding: 0 10rpx;
	}
</style>
