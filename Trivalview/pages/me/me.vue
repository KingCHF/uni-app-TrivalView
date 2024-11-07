<template>
	<!-- // -->
	<view class="userCenter">
		<view class="userCard">
			<view class="user-box" style="padding-bottom: 10px;">
				<view class="user-img" @click="changeUser">
					<u-avatar :src="pic" size="80"></u-avatar>					
				</view>
				<view class="user-info">
					<view class=" ">{{userName}}</view>
					<view class="user-info-phone">{{phone}}</view>
				</view>
				<view class="sign">
					<!-- <text>签到</text> -->
				</view>
				<view class="sign-icon">
					<u-icon name="arrow-right" color="#fff" size="20"></u-icon>
				</view>
			</view>
		</view>
		
		<view class="overview ">
			<u-cell-group>
				<u-cell icon="coupon" title="我的订单" isLink :iconStyle="iconStyle"></u-cell>
				<u-cell icon="star" title="我的积分" isLink  :iconStyle="iconStyle"></u-cell>
				<u-cell icon="shopping-cart" title="积分兑换" isLink :iconStyle="iconStyle"></u-cell>
				<u-cell icon="heart" title="我的收藏" isLink :iconStyle="iconStyle"></u-cell>
				<u-cell icon="setting" title="系统设置" isLink :iconStyle="iconStyle"></u-cell>
			</u-cell-group>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				pic:'../../static/avatar.png',
				show:true,
				userName: '小甜鱼',
				phone: '',
				orderTimes: 23,
				revenue: 100,
				totalScore: 100,
				iconStyle:{
					padding: '8px 8px'
				}
			}
		},
		onShow() {
			this.show = uni.getStorageSync('info') != '';
			if(this.show){
				this.userName = uni.getStorageSync('info').nickName;
				this.phone = uni.getStorageSync('info').phoneNum.substr(0,7)+'****';
				console.log(uni.getStorageSync('info').avatar);
				this.pic = uni.getStorageSync('info').avatar != '' ? this.$baseUrl.baseUrl + uni.getStorageSync('info').avatar : 'https://uviewui.com/common/logo.png';
			    
			}else{
				this.userName ="未登录",
				this.phone = '';
				this.orderTimes=0;
				this.revenue=0;
				this.totalScore= 0;
			}
			
		},
		methods: {
			changeUser(){
				uni.navigateTo({
					url:'/pages/login/login'
				})
			}
			//如果有积分、回收次数等的需求，可以添加功能。这里省略此部分。
		}
	}
</script>

<style lang="scss">
page{
	background-color: #ededed;
}


.user-box{
	padding-top: 10px;
	color: #fff;
	display: flex;
	padding: 30 10 20 30;
	align-items: center;
}

.userCenter{
	box-sizing: border-box;
	padding: 0 15px;
	background-color: #fff;
	overflow: hidden;
}
.userCard{
	margin: 15px auto ;
	background-color: #71d9ca;
	border-radius: 10px;
	padding: 10px;
	margin-bottom: 0px;
}
.user-info{
	flex:1;
	font-size: 18px;
	margin-left: 15px;
}
.user-info-phone{
	font-size: 15px;
}
.sign{
	font-size: 15px;
	margin-right: 15px;
}

.overview{
	display: flex;
	align-items: center;
	padding-top: 15px;
}
.view-item{
	flex: 1;
	font-size: 16px;
	text-align: center;
	color: #fff;
	line-height: 24px;
}
.userDetail{
	padding-top: 20px;
	padding-bottom: 20px;
}
.iconStyle{
	padding-top: 50px;
}
</style>

