<template>
	<view>
		<h1>您已成功选择套餐</h1>
		<h1>You've selected Plan Successfully</h1>
		
		<image src='../../static/cheer.png'></image>
		
		<button type="default" @tap='gotoHome'>返回主页</button>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		onLoad() {
			my.setStorage({
				key: 'isPayed',
				data:'unpayed',
				  success: function() { 
					  console.log('ok')
				  }
			})
		},
		methods: {
			gotoHome(){
				uni.navigateTo({
				  
				    url:"../user/user"
							
				})
			}
		}
	}
</script>

<style scoped>
h1{
		color:#DA3E00;
		font-size:54rpx;
	}
</style>
