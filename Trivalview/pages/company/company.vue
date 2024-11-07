<template>
	<!-- // -->
	<view id="companyRecycle">
		<view class="cpHead">
			<view class="title">
				<image mode="widthFix" src="../../static/app_logo.png" style="width: 20px;height: 10px;padding:10px 10px 0px 10px;"></image>
				<text> 旅游公司介绍</text>
			</view>
			<view class="search">
				<u-search placeholder="请输入搜索内容" height="30" shape="square" v-model="keyword" @search="search"  :showAction="false" ></u-search>
			</view>
		</view>
		<view class="container">
			<scroll-view scroll-y="true" class="companylist">
				<view class="company-item" v-for=" (item,index) in cpnlist" :key="index">
					<view class="item-title">{{item.name}}</view>
					<view class="item-cont"  @click="gotoDetail(item.id)" >
						<view class="item-img">
							<image :src="$baseUrl.baseUrl+item.coverUrl"></image>
						</view>
						<view class="cont-right">
							<view class="right-info">{{item.address}}</view>
							<view class="right-info">营业时间：8:00-16:00</view>
							<view class="right-info">联系电话：{{item.contact}}</view>
						</view>
					</view>
					<view class="placeOrder" style="width: 100%;">
						<u-button @click="gotoFillIn(item.id)" type="primary"  color="#4EEE94">立即参团</u-button>
					</view>
				</view>
			</scroll-view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				cpnlist: [],
				keyword: '',
			}
		},
		onLoad() {
			this.getCompantlist();
		},
		methods: {
			getCompantlist() {
				uni.$u.http.get(this.$baseUrl.baseUrl+'/companys', {}).then(res => {
					console.log(res);
					if (res.statusCode == 200) {
						this.cpnlist = res.data;
						console.log(this.cpnlist);
					}
				})
			},
			gotoDetail(id) {
				uni.navigateTo({
					url: '/childPages/company/detail?id=' + id
				})
			},
			search(e) {
				uni.navigateTo({
					url: '/childPages/company/searchRes?text=' + e
				})
			},
			gotoFillIn(id,name) {
				uni.navigateTo({
					url: '/childPages/company/fillIn?companyId=' + id+'&companyName='+name
				})
			}
		}
	}
</script>

<style scoped>
	#companyRecycle {
		background-color: #F7fafb;
	}

	.cpHead {
		box-sizing: border-box;
		padding: 0px 20px;
		background-color: #4EEE94;
		height: 110px;
	}

	.cpHead .title {
		font-size: 20px;
		color: #fff;
		display: flex;
		align-items: center;
	}

	.cpHead .title image {
		width: 60px;
	}

	.search {
		width: 100%;
		box-sizing: border-box;
		padding: 10px 20px 0px 20px;
		background-color: #4EEE94;
	}

	.container {
		width: 100%;
		height: calc(100vh - 250px);
		overflow: hidden;
	}

	.companylist {
		box-sizing: border-box;
		padding: 0 20px;
		height: 100%;
	}

	.company-item {
		box-sizing: border-box;
		padding: 10px;
		margin: 15px  5px 0 5px;
		background-color: #fff;
		border-radius: 8px;
		box-shadow: 0 1px 8px #e9e9e9;
	}

	.company-item::after {
		content: '';
		display: block;
		clear: both;
	}

	.item-title {
		font-size: 18px;
		border-bottom: 1rpx #e6e6e6 solid;
		line-height: 36px;
	}

	.item-cont {
		display: flex;
		padding: 5px 0;
	}

	.item-img {
		flex: .8;
	}

	.item-img image {
		width: 100%;
		height: 90px;
	}

	.cont-right {
		flex: 2;
		margin-left: 10px;
		display: flex;
		flex-flow: column;
		justify-content: space-between;
	}

	.placeOrder {
		width: 30%;
		float: right;
	}
</style>
