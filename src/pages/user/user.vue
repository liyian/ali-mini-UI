<template>
	<view class='user-page'>
		<div class='user-header'>
			<div style='padding-left:36px;'>
			<image src='../../static/logo_EDF_BLANC.png'></image>
			<h1>EDF 电费缴纳系统</h1>
			<h2>贴心、安全、一键缴费</h2>
			</div>
		</div>
		
		<div class='user-info'>
			<div class='user-info-top'>
				<image :src="info.avatar"></image>
				<div class='name'>
					<p>{{info.nickName}}</p>
					<p style="color:#3B3C3C;font-size: 11px;font-weight: 400;">{{address}}</p>
					<p style="color:#DA3E00;font-size: 13px;font-weight: 400;">编辑关联地址信息</p>
				</div>
			</div>
			<div class='user-info-middle'>
				<image src='../../static/none.png'></image>
				<div>
					<p>未选择套餐</p>
					<p>请点击上方选择套餐</p>
				</div>
			</div>
			<div class='user-info-bottom'>
				<button class='history-button'>查看历史付费</button>
				<button class='pay-button' @click='payment'>现在缴费</button>
			</div>
		</div>
		
		<div class='user-feature'>
			<h1>其他功能</h1>
		</div>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				info:{},
				address:'未关联地址'
			}
		},
		onLoad(options){
			this.info = JSON.parse(options.info)
			console.log(this.info)
		},
		methods: {
			async payment(){
				var _this = this
				var code = await this.getAuthCode()
				
				my.request({
				  url: 'http://localhost:5757/aliPay/oauth',
				  method: 'POST',
				  data: {//data里的key、value是开发者自定义的
				    code: _this.authCode
				  },
				  dataType: 'json',
				  success: function(res) {
				    my.alert({content: 'success'});
				  },
				  fail: function(res) {
				    my.alert({content: 'fail'});
				  },
				  complete: function(res) {
				    my.hideLoading();
				    my.alert({content: 'complete'});
				  }
				});
			},
			async getAuthCode(){
				var _this = this
				await my.getAuthCode({
				  scopes: 'auth_user',
				  success: (res) => {
					_this.authCode =res.authCode
					console.log(_this.authCode)
					uni.showToast({
						title:'登陆中...'
					})
					
				  },
				})
			},
		}
	}
</script>

<style lang="scss">
p{
	color:#0A1C53;
	text-align:left;
}
.user-page{
	background:#EDEFF2;
	height:100vh;
	display: flex;
	flex-direction: column;
	align-items: center;
	.user-info{
		padding:20px;
		background:white;
		border-radius: 5px;
		width:80%;
		height:50%;
		position:absolute;
		top:20%;
		image{
			width:50px;
			height: 50px;
			border-radius: 60px;
		}
		.user-info-top{
			height:30%;
			line-height: 23px;
			display:flex;width:100%;padding:10px;border-bottom:1px solid #D8D8D8;
		}
		.user-info-middle{
			height:40%;
			display: flex;
			image{
				width: 40%;
				height: 90%;
				margin: 20px;
			}
			div{
				width: 45%;
				font-size:12px;
				display: flex;
				flex-direction: column;
				align-items: center;
				justify-content: center;
			}
		}
		.user-info-bottom{
			padding-top:10px;
			height:30%;
			display:flex;
			align-items: center;
			justify-content: center;
			button{
				width:40%;
				border-radius: 5px;
				font-weight: bold;
			}
			.history-button{
				background:#E4E8F0;
				color:#0A1C53;
				margin-right:20px;
			}
			.pay-button{
				background:#FBEBE5;
				color:#DF5420;
			}
		}
	}
	.user-feature{
		background:white;
		padding:20px;
		border-radius: 5px;
		width:80%;
		top:80%;
		height:10%;
		position:absolute;
	}
	.user-header{
		width: 100%;
		height:30%;
		background:#273262;
		border-radius: 0 0 25px 20px;
		image{
			width: 100px;
			height: 50px;
		}
		h1{
			color:white;
			font-weight: bold;
			font-size:27px;
		}
		h2{
			color:white;
		}
	}
}
</style>
