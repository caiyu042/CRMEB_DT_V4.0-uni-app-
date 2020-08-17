<template>
	<view>
		<!-- #ifdef H5 -->
		<view v-for="(item, index) in styleConfig" :key="index" @click="bindParent(item)">
			<component
				:is="item.name"
				:index="index"
				:activeName="activeName"
				:dataConfig="item"
				@changeBarg="changeBarg"
				@changeTab="changeTab"
				:tempArr="tempArr"
				:iSshowH="iSshowH"
				@detail="goDetail"
				@bindIframe="bindIframe"
			></component>
		</view>
		<!-- #endif -->
		<!-- #ifdef MP -->
		<block v-for="(item,index) in styleConfig" :key='index'>
			<a_headerSerch v-if='item.name=="a_headerSerch"' :dataConfig="item"></a_headerSerch>
			<b_swiperBg v-if='item.name=="b_swiperBg"' :dataConfig="item"></b_swiperBg>
			<c_menus v-if='item.name=="c_menus"' :dataConfig="item"></c_menus>
			<d_news v-if='item.name=="d_news"' :dataConfig="item"></d_news>
			<e_activity v-if='item.name=="e_activity"' :dataConfig="item"></e_activity>
			<k_live v-if='item.name=="k_live"' :dataConfig="item"></k_live>
			<f_scroll_box v-if='item.name=="f_scroll_box"' :dataConfig="item"></f_scroll_box>
			<g_recommend v-if='item.name=="g_recommend"' :dataConfig="item"></g_recommend>
			<h_popular v-if='item.name=="h_popular"' :dataConfig="item"></h_popular>
			<i_m_banner v-if='item.name=="i_m_banner"' :dataConfig="item"></i_m_banner>
			<i_new_goods v-if='item.name=="i_new_goods"' :dataConfig="item"></i_new_goods>
			<j_promotion v-if='item.name=="j_promotion"' :dataConfig="item"></j_promotion>
		</block>
		<!-- #endif -->
		<view class="loadingicon acea-row row-center-wrapper" v-if="tempArr.length && styleConfig[styleConfig.length - 1].name == 'promotionList'">
			<text class="loading iconfont icon-jiazai" :hidden="loading == false"></text>
			{{ loadTitle }}
		</view>
		<!-- #ifdef MP -->
		<authorize @onLoadFun="onLoadFun" :isAuto="isAuto" :isShowAuth="isShowAuth" @authColse="authColse" :isGoIndex="false"></authorize>
		<!-- #endif -->
	</view>
</template>

<script>
// #ifdef H5
import mConfig from './components/index.js';
import { getShare } from '@/api/public.js';
// #endif
// #ifdef MP
import {
		SUBSCRIBE_MESSAGE,
		TIPS_KEY
	} from '@/config/cache';
import a_headerSerch from './components/a_headerSerch';
import b_swiperBg from './components/b_swiperBg';
import c_menus from './components/c_menus';
import d_news from './components/d_news';
import e_activity from './components/e_activity';
import f_scroll_box from './components/f_scroll_box';
import g_recommend from './components/g_recommend';
import h_popular from './components/h_popular';
import i_m_banner from './components/i_m_banner';
import i_new_goods from './components/i_new_goods';
import j_promotion from './components/j_promotion';
import k_live from './components/k_live'
import authorize from '@/components/Authorize.vue';
import { getTemlIds } from '@/api/api.js';
// #endif
import { mapGetters } from 'vuex';
import { getDiy, getIndexData } from '@/api/api.js';
import { getGroomList } from '@/api/store.js';
import { goShopDetail } from '@/libs/order.js';
import { toLogin } from '@/libs/login.js';
let app = getApp();
export default {
	computed: mapGetters(['isLogin', 'uid']),
	components: {
		// #ifdef H5
		...mConfig,
		// #endif
		// #ifdef MP
		authorize,
		a_headerSerch,
		b_swiperBg,
		c_menus,
		d_news,
		e_activity,
		f_scroll_box,
		g_recommend,
		h_popular,
		i_m_banner,
		i_new_goods,
		j_promotion,
		k_live,
		authorize
		// #endif
	},
	data() {
		return {
			styleConfig: [],
			tempArr: [],
			goodType: 3,
			loading: false,
			loadend: false,
			loadTitle: '下拉加载更多', //提示语
			page: 1,
			limit: this.$config.LIMIT,
			iSshowH: false,
			numConfig: 0,
			isIframe: false,
			activeName: ''
		};
	},
	onLoad(options) {
		uni.getLocation({
			type: 'wgs84',
			success: function(res) {
				try {
					uni.setStorageSync('user_latitude', res.latitude);
					uni.setStorageSync('user_longitude', res.longitude);
				} catch {}
			}
		});
		this.diyData();
		this.getIndexData();
		// #ifdef H5
		this.setOpenShare();
		window.addEventListener('message', this.handleMessageFromParent, false);
		if (app.globalData.isIframe) {
			uni.hideTabBar();
		}
		// #endif
		// #ifdef MP
		this.getTemlIds();
		// #endif
	},
	onShow() {},
	mounted() {},
	methods: {
		// #ifdef MP
		getTemlIds() {
			let messageTmplIds = wx.getStorageSync(SUBSCRIBE_MESSAGE);
			if (!messageTmplIds) {
				getTemlIds().then(res => {
					if (res.data) wx.setStorageSync(SUBSCRIBE_MESSAGE, JSON.stringify(res.data));
				});
			}
		},
		// #endif
		onLoadFun() {},
		bindParent(item) {
			this.activeName = item.name;
			if (app.globalData.isIframe) {
				window.parent.postMessage(
					{
						name: item.name,
						params: {}
					},
					'*'
				);
				return;
			}
		},
		// #ifdef H5
		handleMessageFromParent(event) {
			var data = event.data;
			// console.log(event.data,'handleMessageFromParent')
		},
		// #endif
		// 对象转数组
		objToArr(data) {
			let obj = Object.keys(data);

			let m = obj.map(function(key) {
				data[key].name = key;
				return data[key];
			});
			return m;
		},
		diyData() {
			let that = this;
			getDiy().then(res => {
				try {
					if (res.data.a_headerSerch.hotList.list.length > 0) {
						uni.setStorageSync('hotList', res.data.a_headerSerch.hotList.list);
					}
				} catch (err) {}
				that.styleConfig = that.objToArr(res.data);
				// #ifdef MP
				let obj = {}
				obj.name = 'k_live'
				that.styleConfig.splice(5,0,obj)
				// #endif
			});
		},
		getIndexData() {
			let self = this;
			getIndexData().then(res => {
				self.$store.commit('indexData/setIndexData', res.data);
			});
		},
		// #ifdef H5
		// 微信分享；
		setOpenShare: function() {
			let that = this;
			if (that.$wechat.isWeixin()) {
				getShare().then(res => {
					let data = res.data.data;
					let configAppMessage = {
						desc: data.synopsis,
						title: data.title,
						link: location.href,
						imgUrl: data.img
					};
					that.$wechat.wechatEvevt(['updateAppMessageShareData', 'updateTimelineShareData'], configAppMessage);
				});
			}
		}
		// #endif
	},
	onReachBottom: function() {
		// this.getGroomList();
	}
};
</script>

<style></style>
