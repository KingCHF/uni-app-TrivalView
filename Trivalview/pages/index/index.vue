<template>
	<view>
		<!-- 输入框 -->
		<view class="search usearch" style="background-color: #4EEE94;">
			<u-search placeholder="请输入搜索内容"  height="30" shape="round" v-model="keyword" @search="search"
				:showAction="false"></u-search>
		</view>
		
		<!-- 通知栏 -->
		<view>
		    <u-notice-bar :text="text1" style="height: 10px;"></u-notice-bar>
		</view>
		
		<!-- 轮播 -->
		<u-swiper
		        :list="list1"
		        @change="change"
		        @click="click"
				style="padding: 10px 10px;background-color: white;border-radius: 0%;"
		></u-swiper>
		
		<!-- 卡片 -->
		<view style="padding: 0px 10px;">
           <u-grid
                :border="false"
                col="4"
        >
            <u-grid-item
                    v-for="(listItem,listIndex) in list"
                    :key="listIndex"
            >
                <u-icon
                        :customStyle="{paddingTop:20+'rpx'}"
                        :name="listItem.name"
                        :size="22"
                ></u-icon>
                <text class="grid-text">{{listItem.title}}</text>
            </u-grid-item>
        </u-grid>
            <u-toast ref="uToast" />
        </view>

		
		<view class="btnOrder">
			<u-button color="#4EEE94" size="large" shape="circle" @click="goto(5)">点击报名旅游团</u-button>
		</view>
		
		
		
		<!-- 卡片 -->
		
		<view class="activeWrap">
			<view style="display: flex;">
				<view class="title" style="padding-right: 30px;"><u-link href="https://baike.baidu.com/item/%E8%80%81%E5%90%9B%E5%B1%B1/22174?fr=ge_ala" text="老君山" color="black" line-color="#19be6b"></u-link></view>
				<u-rate :value="5" readonly></u-rate>
			</view>
			<image src="../../static/ljs.jpg" style="height: 150px;width: 100%;"></image>
			<view class="text-clamp" style="text-indent: 2rem;font-size: 14px;">
				老君山（The Laojun Mountain），“天下无双圣境，世界第一仙山”。位于十三朝古都洛阳的栾川县县城东南三千米处。是秦岭余脉八百里伏牛山的主峰，海拔2200米，原名景室山。
			</view>
		</view>
		
		<u-divider text="分割线" :dot="true" style="padding: 0px 20px;"></u-divider>
		
		<view class="activeWrap">
			<view style="display: flex;">
				<view class="title" style="padding-right: 20px;"><u-link href="https://baike.baidu.com/item/%E9%A6%99%E6%A0%BC%E9%87%8C%E6%8B%89%E5%B8%82/16803537?fr=ge_ala" text="香格里拉" color="black" line-color="#19be6b"></u-link></view>
				<u-rate :value="5" readonly></u-rate>
			</view>
			<image src="../../static/xgll.jpg" style="height: 150px;width: 100%;"></image>
			<view class="text-clamp" style="text-indent: 2rem;font-size: 14px;">
                 香格里拉市（Shangri-la），云南省迪庆藏族自治州辖县级市，藏语意为“心中的日月”，位于云南省西北部、青藏高原横断山区腹地，是滇、川、藏三省区交界地；</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list: [{
                    name: 'order',
                    title: '酒店预订'
                    },
                    {
                        name: 'shopping-cart',
                        title: '商品购物'
                    },
                    {
                        name: 'email',
                        title: '信息查询'
                    },
                    {
                        name: 'rmb-circle',
                        title: '大额优惠'
                    },
                    {
                        name: 'coupon',
                        title: '景点门票'
                    },
                    {
                        name: 'calendar',
                        title: '车站大屏'
                    },
					{
					    name: 'car',
					    title: '行李托运'
					},
					{
					    name: 'kefu-ermai',
					    title: '联系客服'
					},
                ],
				swiperlist: [],
				keyword: '',
				activelist: [],
				list1: [
				        '/static/01.jpg',
				        '/static/02.jpg',
				        '/static/03.jpg',
				        ],
				text1: '老君山风景区 11 月 6 日 - 11 月 20 日对全国大学生实行免门票政策!',
			}
		},
		onLoad() {
			this.getHotDots();
			this.getActlist();
		},
		methods: {
			getHotDots() {
				uni.request({
					url: this.$baseUrl.baseUrl + '/hotDots',
					methods: 'get',
					success: (res) => {
						console.log(res.data);
						this.swiperlist = res.data;
						for (let i = 0; i < this.swiperlist.length; i++) {
							this.swiperlist[i].image = this.$baseUrl.baseUrl + this.swiperlist[i].image;
						}
					}
				})
			},
			search(e) {
				console.log(e);
				uni.navigateTo({
					url: '/childPages/type/searchRes?txtSearch=' + e
				});
				this.keyword = "";
			},
			goto(i) {
				switch (i) {
					case 1:
						uni.navigateTo({
							url: '/childPages/type/recyclingMenu'
						});
						break;
					case 5:
						uni.switchTab({
							url:'/pages/company/company'
						})
						break;
					default:
						break;
				}
				},
			getActlist() {
				uni.request({
					url: this.$baseUrl.baseUrl + '/actions',
					methods: "get",
					success: (res) => {
						console.log(res);
						this.activelist = res.data;
						console.log(this.activelist);
					}
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.search {
		width: 100%;
		box-sizing: border-box;
		padding: 20px 20px 15px 20px;
		background-color: rgb(0, 194, 151);
	}

	.swiper {
		padding: 10px;
		background-color: #F4F4F5;
	}

	.char {
		margin-top: 20px;
		margin-left: 10rpx;
		display: flex;
		justify-content: space-around;
	}

	.char_item {
		flex: 1;
		text-align: center;
		font-size: 14px;
		color: #888888;
	}

	.char_item image {
		width: 60%;
	}

	.btnOrder {
		padding: 20px;
	}

	.char_m {
		margin-top: 0;
	}

	.activeWrap {
		padding: 0 20px;
	}

	.activeWrap .title {
		font-size: 16px;
		padding-left: 10px;
		border-left: 3px solid rgb(0, 194, 151);
		margin-top: 10px;
		margin-bottom: 10px;
	}

	.activeWrap .actlist {
		width: 100%;
	}

	.act_item {
		margin-bottom: 10px;
	}
	.usearch {
	  border-top: 1px solid white; /* 只保留上边框 */
	  border-right: none;  /* 取消右边框 */
	  border-bottom: none; /* 取消下边框 */
	  border-left: none;   /* 取消左边框 */
	}
	
	.text-clamp {
	  font-size: 14px;
	  text-indent: 2rem; /* 首行缩进2rem，rem是相对于根元素（html）的字体大小的单位 */
	  display: -webkit-box;
	  -webkit-box-orient: vertical;
	  -webkit-line-clamp: 3; /* 限制显示的行数为3行 */
	  overflow: hidden; /* 隐藏超出部分的内容 */
	  text-overflow: ellipsis; /* 超出部分显示省略号 */
	  line-height: 1.5; /* 根据需要调整行高 */
	  width: 100%; /* 或者你需要的具体宽度 */
	}
	
	.grid-text {
	        font-size: 14px;
	        color: #909399;
	        padding: 10rpx 0 20rpx 0rpx;
	        /* #ifndef APP-PLUS */
	        box-sizing: border-box;
	        /* #endif */
	    }
</style>
