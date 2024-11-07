<template>
	<!-- // -->
	<view>
		<u-form  :model="form" ref="uForm" label-width="80" labelPosition="left">
		<u-form-item label="物品重量" prop="weight" borderBottom>
				<u-input v-model="form.weight" placeholder="请输入" border="none"></u-input>
			</u-form-item>			
		</u-form>
	{{form.weight}}
	</view>
</template>

<script>
	export default {
		data() {
			return {
				form: {					
						weight:''				
					}					
				}
		},
		methods: {			
		}
	}
</script>

<style>

</style>
