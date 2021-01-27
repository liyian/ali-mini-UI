<template>
	<view class='begin'>
		<div class='begin-body'>
			<image src='../../static/logo_EDF.png'></image>
			<h1>EDF法国电力</h1>
			<h1>缴电服务</h1>
			<h2>贴心、安全、一键缴费</h2>
			<button class='begin-button' @getAuthorize="onGetAuthorize" scope="userInfo" open-type="getAuthorize">开始使用</button>
		</div>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			};
		},
		onLoad(){
			my.clearStorage();
		},
		methods:{
			onGetAuthorize(res) {
			    my.getOpenUserInfo({
			        fail: (res) => {},
			        success: (res) => {
			            let userInfo = JSON.parse(res.response).response // 以下方的报文格式解析两层 response
			            console.log(userInfo);
			            uni.navigateTo({
							url: '../user/user?info='+JSON.stringify(userInfo)
			            })
			        },
			    });			
			},
		}
	}
</script>

<style lang="scss">
h1{
	font-size:27px;
	font-weight: 400;
	
}
h2{
	font-size: 18px;
}
.begin{
	line-height: 60px;
	height:100vh;
	display:flex;
	align-items:center;
	flex-direction: column;
	justify-content: center;
	.begin-body{
		width: 100%;
		display:flex;
		align-items:center;
		flex-direction: column;
		justify-content: center;
	}
	image{
		width:190px;
		height:110px;
	}
	.begin-button{
		background:#DA3E00;
		color:white;
		width:80%;
		border-radius: 10px;
	}
}
</style>
