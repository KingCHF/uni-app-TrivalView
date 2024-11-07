<template>
	<!-- // -->
	<view>
		<view class="search">
			<u-search placeholder="请输入搜索内容" style="padding-left: 40px;" shape="square" v-model="keyword" @search="search" actionText="":showAction="true"></u-search>
		</view>
		<view class="container">
			<scroll-view scroll-y="true" class="companylist" v-if="cpnlist.length > 0">
				<view class="company-item" v-for="(item,index) in cpnlist" :key="index" @click="gotoDetail(item.id)">
					<view class="item-title">{{item.name}}</view>
					<view class="item-cont">
						<view class="item-img">
							<image :src="$baseUrl.baseUrl + item.coverUrl"></image>
						</view>
						<view class="cont-right">
							<view class="right-info">{{item.address}}</view>
							<view class="right-info">营业时间：8:00-16:00</view>
							<view class="right-info">联系电话：{{item.contact}}</view>
						</view>
					</view>
					<view class="placeOrder" style="width: 100%;">
						<u-button :customStyle="customStyle" type="primary" color="#4EEE94">立即参团</u-button>
					</view>
				</view>
			</scroll-view>
			<view class=" emptyBox" v-else>
				<u-empty text="暂无搜索数据!" mode="data"></u-empty>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				cpnlist: [],
				keyword: ''
			}
		},
		onLoad(options) {
			this.keyword = options.text;
			this.getSearchRes(options.text)
		},
		methods: {
			gotoDetail(id) {
				uni.navigateTo({
					url: '/childPages/company/detail?id=' + id
				})
			},
			search(e) {
				this.getSearchRes(e);
			},
			getSearchRes(str) {
				uni.$u.http.get(this.$baseUrl.baseUrl + "/companys?name_like=" + str, {

				}).then(res => {
					console.log(res);
					if (res.statusCode == 200) {
						this.cpnlist = res.data;
						console.log(this.cpnlist);
					}
				})
			}
		}
	}
</script>

<style scoped>
	.customStyle {
		backgroundColor: '#4EEE94';
		color: 'white';
		height: '32px';
		fontSize: '14px';
		borderRadius: '16px'
	}
	.result {
		background-color: #f7fafd;
	}
	.search {
		width: 100%;
		box-sizing: border-box;
		padding: 15px 20px 15px 20px;
		background-color: #4EEE94;
	}
	.container {
		width: 100%;
		height: 600px;
		height: calc(100vh - 130px);
		overflow: hidden;
	}
	.companylist {
		box-sizing: border-box;
		padding: 0 15px;
		height: 100%;
	}
	.company-item {
		box-sizing: border-box;
		padding: 10px;
		margin: 15px 5px 0 5px;
		background-color: #fff;
		border-radius: 8px;
		box-shadow: 0 1px 8px #e7e7e7;
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
		margin-top: 5px;
		float: right;
	}
	.emptyBox {
		position: fixed;
		top: 50%;
		left: 50%;
		transform: translate3d(-50%, -50%, 0);
	}
</style>
