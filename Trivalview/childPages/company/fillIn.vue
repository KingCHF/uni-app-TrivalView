<template>
	<!-- // -->
	<view class="formWrap">
		<u-form :model="form" ref="uForm" label-width="80" labelPosition="left" :rules="rules">
			<u-form-item label="联系人" prop="order.name" borderBottom>
				<u-input v-model="form.order.name" placeholder="请输入" border="none"></u-input>
			</u-form-item>
			<u-form-item label="联系方式" prop="order.phone" borderBottom>
				<u-input v-model="form.order.phone" placeholder="请输入" border="none"></u-input>
			</u-form-item>
			<u-form-item label="所在城市" prop="order.address" borderBottom>
				<u-input v-model="form.order.address" placeholder="请输入" border="none"></u-input>
			</u-form-item>
			<u-form-item label="预约时间" prop="order.time" borderBottom @click="timeShow = true">
				<u-input v-model="form.order.time" placeholder="请选择预约时间" border="none" disabled disabledColor="#ffffff">
				</u-input>
				<u-icon slot="right" name="arrow-right"></u-icon>
				<u-action-sheet :show="timeShow" :actions="timelist" title="请选择时间" description="选择预约时间"
					@close="timeShow = false" @select="timeCfm">
				</u-action-sheet>
			</u-form-item>
			<u-form-item label="行李重量" prop="order.weight" borderBottom>
				<u-input v-model="form.order.weight" placeholder="请输入" border="none"></u-input>
			</u-form-item>
			<u-form-item label="行李件数" prop="order.number" borderBottom>
				<u-input v-model="form.order.number" placeholder="请输入" border="none"></u-input>
			</u-form-item>
		</u-form>



		<view style="margin: 15px auto;">
			<u-upload :fileList="fileList" @afterRead="afterRead" @delete="deletePic" name="2" multiple :maxCount="10">
			</u-upload>
		</view>
		<view class="details">
			<textarea v-model="content" placeholder="请输入其他信息"></textarea>
		</view>
		<view class="appointBtn">
			<u-button type="primary" @click="appointment" color="#4EEE94">立即预约</u-button>
		</view>
		<u-toast ref="uToast"></u-toast>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				fileList: [],
				latitude: 39.909,
				longitude: 116.39742,
				form: {
					order: {
						type: '',
						name: '',
						phone: '',
						address: '',
						time: '',
						weight: '',
						number: ''
					}
				},
				rules: {
					'order.type': [{
						required: true,
						message: '请选择物品类别',
						trigger: ['change']
					}],
					'order.name': [{
						required: true,
						message: '请输入联系人',
						trigger: ['blur']
					}],
					'order.phone': [{
						required: true,
						type: 'number',
						message: '请输入联系方式',
						trigger: ['blur']
					}],
					'order.address': [{
						required: true,
						message: '请输入地址',
						trigger: ['blur']
					}],
					'order.time': [{
						required: true,
						message: '请选择上门时间',
						trigger: ['change']
					}],
					'order.weight': [{
						required: true,
						type: 'float',
						message: '请输入物品重量',
						trigger: ['blur']
					}],
					'order.number': [{
						required: true,
						message: '请输入物品件数',
						type: 'integer',
						trigger: ['blur']
					}],
				},
				typeShow: false,
				timeShow: false,
				typelist: [
					['废纸', '塑料', '金属', '纺织品', '家电'],
					['报纸', '书籍', '纸箱']
				],
				typeData: [
					['报纸', '书籍', '纸箱'],
					['水瓶', '桶', '塑料袋', '农用地膜', '泡沫塑料', '塑料编织品'],
					['铜丝', '铁制品', '废钢筋', '不锈钢', '废银锌电池'],
					['羽绒服', '毛衣', '棉絮', '上衣', '裤子', '裙子'],
					['电视机', '空调', '冰箱', '洗衣机', '电脑', '微波炉', '烤箱']
				],
				typeId: null,
				unitPrice: null,
				timelist: [{
					name: '8:00-10:00'
				}, {
					name: '10:00-12:00'
				}, {
					name: '14:00-16:00'
				}, {
					name: '16:00-18:00'
				}],
				photoUrl: '',
				content: '',
				companyId: null
			}
		},
		onReady() {
			this.$refs.uForm.setRules(this.rules);
		},
		onLoad(options) {
			let user = uni.getStorageSync('info');
			if (user == '') {
				uni.navigateTo({
					url: '/pages/login/login'
				})
			}
			this.companyId = options.id;
			this.companyName = options.Name;
		},
		methods: {
			changeHandler(e) {
				const {
					columnIndex,
					value,
					values, // values为当前变化列的数组内容
					index,
					// 微信小程序无法将picker实例传出来，只能通过ref操作
					picker = this.$refs.uPicker
				} = e
				// 当第一列值发生变化时，变化第二列(后一列)对应的选项
				if (columnIndex == 0) {
					// picker为选择器this实例，变化第二列对应的选项
					picker.setColumnValues(1, this.typeData[index])
				}
			},

			typeCancel() {
				this.typeShow = false;
			},
			typeCfm(e) {
				console.log(e);
				console.log(e.value[1]);
				this.form.order.type = e.value[1];
				this.typeShow = false;
			},
			timeCfm(e) {
				console.log(e.name);
				this.form.order.time = e.name;
			},
			appointment() {

				let userId = uni.getStorageSync('info').id;
				this.photoUrl = '';
				this.unitPrice = 2.3;
				this.$refs.uForm.validate().then(valid => {
					console.log("valid=" + valid);
					if (valid) {
						uni.request({
							url: this.$baseUrl.baseUrl + '/orders',
							method: 'POST',
							data: {
								"companyName": this.companyName,
								"createby": this.form.order.name, //联系人
								"createTime": this.$u.timeFormat(new Date(), "yyyy-mm-dd hh:MM:ss"),
								"pickupTime": this.form.order.time, //取货时间
								"pickupAddress": this.form.order.address, //取货地址
								"goodNumber": this.form.order.weight, //重量
								"goodUnitPrice": this.unitPrice, //回收物品单位价格
								"goodTypeName": this.form.order.type, //回收类型名称
								"coverUrl": "/image/order/paper.jpeg",
								"contact": this.form.order.phone, //联系电话
								"remark": this.content, //备注
								"pkgs": this.form.order.weight, //重量,
								"state": 1,
								"companyId": this.companyId,
								"userId": userId
							},
							success: (res) => {
								console.log(res);
								if (res.statusCode == 201) {
									this.$refs.uToast.show({
										message: '预约成功',
										type: 'success',
										isTab: true

									})
									uni.switchTab({
										url: '/pages/order/order'
									})
								} else {
									uni.showToast({
										message: '预约失败，请重试！',
										icon: 'error'
									})
								}
							}
						})
					}
				})
			}
		}
	}
</script>

<style scoped>
	.formWrap {
		box-sizing: border-box;
		padding: 0 15px 70px 15px;
	}

	.details {
		border: 1rpx #E6E6E6 solid;
		border-radius: 5px;
	}

	.appointBtn {
		width: calc(100% - 40px);
		position: fixed;
		left: 20px;
		bottom: 20px;
	}
</style>
