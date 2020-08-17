<template>
	<view class="index-wrapper" :class="{borderShow:isBorader}">
		<view class='wrapper' v-if="benefit.length">
			<view class='title acea-row row-between-wrapper'>
				<view class='text'>
					<view class='name line1'>{{titleInfo[0].val}}</view>
					<view class='line1'>{{titleInfo[1].val}}</view>
				</view>
				<view class='more' @click="gopage('/pages/columnGoods/HotNewGoods/index?type=4')">更多<text class='iconfont icon-jiantou'></text></view>
			</view>
			<promotionGood :benefit="benefit"></promotionGood>
		</view>
	</view>
</template>

<script>
	let app = getApp()
	import { mapState} from 'vuex'
	import { goPage } from '@/libs/order.js'
	import promotionGood from '@/components/promotionGood/index.vue'
	export default {
		name: 'j_promotion',
		props: {
			dataConfig: {
				type: Object,
				default: () => {}
			},
			activeName: {
				type: String,
				default: ''
			},
		},
		components:{
			promotionGood
		},
		computed: {
			...mapState({
		    data:state=>state.indexData.indexDatas.benefit
		  })
		},
		watch:{
			'data': {
					handler (nVal, oVal) {
							this.benefit = nVal
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
				benefit:[],
				salesInfo:"库存商品优惠促销活动",
				titleInfo:this.dataConfig.titleInfo.list[0].chiild,
				isBorader:false,
				name: this.$options.name
			}
		},
		methods:{
			gopage(url){
				goPage().then(res=>{
					uni.navigateTo({
						url:url
					})
				})
			}
		}
	}
</script>

<style>
</style>
