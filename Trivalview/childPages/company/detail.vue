<template>
	<!-- // -->
	<view>
		<view class="cpHead">
			<view class="company-item">
				<view class="item-title">{{companyDetail.name}}</view>
				<view class="item-cont">
					<view class="item-img">
						<image :src="$baseUrl.baseUrl + companyDetail.coverUrl"></image>
					</view>
					<view class="cont-right">
						<view class="right-info">地址：{{companyDetail.address}}</view>
						<view class="right-info">联系电话：{{companyDetail.contact}}</view>
						<view class="right-info">评价等级：{{companyDetail.score}}</view>
					</view>
				</view>
			</view>
		</view>
		<view class="introduce">{{companyDetail.introduction}}</view>
		<view class="appointBtn">
			<u-button @click="gotoFillIn(companyDetail.id)"  color="#4EEE94">立即参团</u-button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {				
				companyDetail: {}
			}
		},
		onLoad(options) {
			this.getCompanyDtl(options.id)
		},
		methods: {
			getCompanyDtl(id) {

				uni.$u.http.get(this.$baseUrl.baseUrl + '/companys?id='+id,{}).then( res => {
					console.log(res);
						if(res.statusCode == 200) {
							this.companyDetail = res.data[0];
							console.log(this.companyDetail);
						}
				})
			},
			gotoFillIn(id) {
				uni.navigateTo({
					url: '/childPages/company/fillIn?id='+id
				})
			}
		}
	}
</script>

<style scoped>
	.cpHead{
		box-sizing: border-box;
		padding: 20px;
		background-color: #4EEE94;
	}
	.item-title{
		font-size: 18px;
		line-height: 36px;
		color: #fff;
	}
	.item-cont{
		display: flex;
		padding: 5px 0;
		margin-top: 10px;
	}
	.item-img{
		flex: .8;
	}
	.item-img image{
		width: 100%;
		height: 90px;
	}
	.cont-right{
		flex: 2;
		margin-left: 10px;
		display: flex;
		flex-flow: column;
		justify-content: space-between;
		color: #fff;
	}
	.introduce{
		box-sizing: border-box;
		padding: 20px;
	}
	.appointBtn{
		width: calc(100% - 40px);
		position: fixed;
		left: 20px;
		bottom: 20px;
	}
</style>

