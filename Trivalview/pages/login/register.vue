<template>
	<!-- // -->
	<view class="registerWrap">
		<!-- 标题 -->
		<view class="title">
			<h4 style="text-align: center;">欢迎使用智程旅行</h4>
		</view>
		<!-- 表单 -->
		<view class="formWrap" style="padding: 0px 20px;">
			<view class="formItem">
				<view class="label">手机号：</view>
				<view class="inputBox">
					<input type="number" v-model="phoneNum" />
				</view>
			</view>
			<view class="formItem">
				<view class="label">用户名：</view>
				<view class="inputBox">
					<input type="text" v-model="nickName" />
				</view>
			</view>
			<view class="formItem">
				<view class="label">密码：</view>
				<view class="inputBox">
					<input password="true" v-model="password" />
				</view>
			</view>
		</view>
		<view style="padding: 0px 20px;">
			<u-button style="border-radius: 5px;" type="primary" @click="submitReg" color="rgb(85, 170, 255)"  >注 册</u-button>
		</view>
		<navigator url="/pages/me/login">
			<view class="registTips" style="font-size: 14px;">已有账号 , 立即登录</view>
		</navigator>
		<u-toast ref='uToast'></u-toast>
	</view>
</template>

<script>
	export default {
		data() {
			return{
				flag: true,
				phoneNum: '',
				nickName: '',
				password: ''
			}
		},
		methods: {
			isSame(){
				
				uni.request({
					url:"http://localhost:3004/users",
					data:{
						phoneNum: this.phoneNum
					},
					success: (res) => {
						if(res.data.length != 0){
							this.flag = false;
							uni.showToast({
								title: '手机号已注册！！',
								icon: 'error'
							})
						}
					}
				});
				console.log(this.flag);
				return this.flag;
			},
			
			submitReg() {
				if (this.phoneNum.length == 0) { //判断手机号
					//提示
					uni.showToast({
						title: '请先输入手机号！',
						icon: 'error'
					})
				} else if (this.phoneNum.length != 11) {
					uni.showToast({
						title: '请输入正确手机号！',
						icon: 'error'
					})
				} else if (this.nickName.length == 0) { //判断昵称
					uni.showToast({
						title: '请输入昵称！',
						icon: 'error'
					})
				} else if (this.password.length == 0) { //判断密码
					uni.showToast({
						title: '请输入密码！',
						icon: 'error'
					})
				} else if(this.isSame()) {					
					//请求
					let URL = this.$baseUrl.baseUrl+'/users';
					uni.request({
						url: URL,
						data: {
							phoneNum: this.phoneNum,
							nickName: this.nickName,
							password: this.password,
							avatar: "/image/avatar/" + Math.ceil(Math.random()*10) + '.jpeg',
							typeid: 1
						},
						method: 'POST',
						header: {
							'content-type': 'application/json',
						},
						success: (res)=>{
							this.$refs.uToast.show({
								title: '登录成功',
								type: 'success'										
							}),
							this.autoLogin()	;					
						},
						fail: function(res) {
							console.log("失败",res)
						}
					});
			}
		},
		autoLogin(){
			uni.request({
				url: this.$baseUrl.baseUrl+'/users',
				method: "GET",
				data: {
					phoneNum: this.phoneNum,
					password: this.password,
					typeId: this.typeid
				},
				success: (res) => {
					console.log(res);
					if (res.statusCode == 200) {
						uni.setStorageSync('info', res.data[0]);
						uni.switchTab({
							url:'/pages/me/me'
						});								
					} 
				}
			});
		}
	}
	}
</script>

<style scoped>
	.registerWrap{
		width: 100%;
		box-sizing: border-box;
		padding: 0 40rpx;
	}
	.title{
		padding-top: 60rpx;
	}
	.title h3{
		font-size: 24px;
		line-height: 40px;
		color: #333;
	}
	.title h4{
		font-size: 18px;
		color: #888;
	}
	.formWrap{
		margin-top: 20px;
	}
	.formItem{
		padding-bottom: 15px;
	}
	.formItem .label{
		font-size: 14px;
		color: #333;
	}
	.inputBox{
		margin-top: 5px;
		border: 1px #c3c3c3 solid;
		border-radius: 8px;
	}
	.inputBox input{
		height: 38px;
		line-height: 38px;
		text-indent: 1em;
	}
	.registTips{
		padding-top: 20px;
		text-align: center;
	
	}
</style>
