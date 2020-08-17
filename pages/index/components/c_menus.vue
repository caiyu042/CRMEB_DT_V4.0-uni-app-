<template>
	<view class='nav acea-row acea-row' :class="{borderShow:isBorader}" v-if="menus.length">
		<block v-for="(item,index) in menus" :key="index">
			<view class='item' @click="menusTap(item.info[1].value)">
				<view class='pictrue'>
					<image :src='item.img'></image>
				</view>
				<view class="menu-txt">{{item.info[0].value}}</view>
			</view>
		</block>
	</view>
</template>

<script>
	let app = getApp()
	import { goPage } from '@/libs/order.js'
	export default {
		name: 'c_menus',
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
		watch: {
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
				menus:this.dataConfig.imgList.list,
				isBorader:false,
				name: this.$options.name
			};
		},
		created() {},
		mounted() {},
		methods: {
			menusTap(url) {
				goPage().then(res=>{
					if(url.indexOf("http") != -1){
						// #ifdef H5
						location.href = url
						// #endif
					}else{
						if(['/pages/goods_cate/goods_cate','/pages/order_addcart/order_addcart','/pages/user/index'].indexOf(url) == -1){
							uni.navigateTo({
								url:url
							})
						}else{
							uni.switchTab({
								url:url
							})
						}
					}
				})
			}
		}
	}
</script>

<style lang="scss">
	.nav {
		background-color: #fff;
		padding-bottom: 30rpx;
		.item {
			margin-top: 30rpx;
			width: 25%;
			text-align: center;
			font-size: 24rpx;

			.pictrue {
				width: 82rpx;
				height: 82rpx;
				margin: 0 auto;

				image {
					width: 100%;
					height: 100%;
					border-radius: 50%;
				}
			}

			.menu-txt {
				margin-top: 15rpx;
			}

			&.four {
				width: 25%;

				.pictrue {
					width: 90rpx;
					height: 90rpx;
				}
			}
		}
	}
</style>
