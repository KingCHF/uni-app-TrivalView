<template>
	<view class="loginWrap">
		<view class="title">
			<h4 style="text-align: center;">欢迎使用智程旅行</h4>
		</view>
		<view class="logo" style="text-align: center;">
			<image src="../../static/logo.jpg" mode="widthFix"></image>
		</view>
		
		<view class="formWrap" style="padding: 0px 20px;">
			<view class="formItem">
				<view class="label">手机号：</view>
				<view class="inputBox">
					<input type="number" v-model="phoneNum" />
				</view>
			</view>
			<view class="formItem">
				<view class="label">密码：</view>
				<view class="inputBox">
					<input type="number" password v-model="password" />
				</view>
			</view>
			<view style="margin-bottom: 10rpx;">
				<u-radio-group v-model="roleVal" placement="row">
					<u-radio  label="记住密码" name="记住密码" :customStyle="{marginRight: '16px'}"  activeColor="rgb(85, 170, 255)"></u-radio>
				</u-radio-group>
			</view>
		</view>
		<view style="padding: 10px 20px;">
			<u-button type="primary" @click="loginSubmit" color="rgb(85, 170, 255)"  >登 录</u-button>
		</view>		
		<navigator url="register1">
			<view style="text-align: center;padding-top: 10px;font-size: 15px;" @tap="gotoReg">没有账号，立即注册</view>
		</navigator>
		<u-toast ref='uToast' />
	</view>
</template>
<script>
	export default {
		data() {
			return {
				phoneNum: '',
				password: '',
				roleVal: '普通用户',
				coverUrl: ''
			}
		},
		methods: {
			gotoReg() {
				uni.navigateTo({
					url: "/pages/login/register"
				})
			},
			loginSubmit() {
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
				} else if (this.password.length == 0) { //判断密码
					uni.showToast({
						title: '请输入密码！',
						icon: 'error'
					})
				} else {
					//请求
					uni.request({
						url: this.$baseUrl.baseUrl+'/users',
						method: "GET",
						data: {
							phoneNum: this.phoneNum,
							password: this.password,
							typeid: this.roleVal == '普通用户' ? 1 : 2
						},
						success: (res) => {							
							if (res.data.length > 0) {
								console.log("成功：", res);
								uni.setStorageSync('info', res.data[0]);
								if (this.roleVal == '普通用户') {
									this.$refs.uToast.show({
										message: '登录成功',
										type: 'success'									
									})
								};
								uni.switchTab({
									url:'/pages/me/me'
								})										
							} else {
								this.$refs.uToast.show({
									message: '无效账号',
									type: 'error'
								})
							}
						}
					});
				}
			}
		}
	}
</script>

<style>
	.loginWrap {
		width: 100%;
		box-sizing: border-box;
		padding: 0 40rpx;
	}
	.logo {
		width: 30%;
		margin: 0 auto;
	}
	.logo image {
		width: 100%;
	}
	.title {
		padding-top: 60rpx;
	}
	.title h3 {
		font-size: 24px;
		line-height: 40px;
		color: #333333;
	}
	.title h4 {
		font-size: 18px;
		color: #888888;
	}
	.formWrap {
		padding-top: 20rpx;
	}
	.formItem {
		padding-bottom: 15px;
	}
	.formItem .label {
		font-size: 14px;
		color: #333333;
	}
	.formItem .inputBox {
		border: 1px #C3C3C3 solid;
		margin-top: 5rpx;
		border-radius: 8px;
	}
	.formItem .inputBox input {
		height: 38px;
		line-height: 38px;
		text-indent: 1em;

	}
	.registTips {
		padding-top: 20px;
		text-align: right;
		color: sandybrown;
	}
</style>
