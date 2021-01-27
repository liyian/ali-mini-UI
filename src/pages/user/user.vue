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
				<image v-if="isPayed === 'unpayed'" src='../../static/unpayed.png'></image>
				<image v-else-if="isPayed === 'payed'"  src='../../static/payed.png'></image>
				<image v-else src='../../static/none.png'></image>
				<div  v-if="isPayed === 'unpayed'">
					<p style='color:#DA3E00;'>存在费用未支付 Waiting for payment</p>
					<p>截止日期 Due: 20.1.1</p>
				</div>
			
				<div v-else-if="isPayed === 'payed'">
					<p>无未支付账单 No Overdue</p>
					<p>下次缴费时间 Next Due：20.1.20</p>
				</div>
				
				<div  v-else>
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
			<div style='padding-top:22px;display: flex; justify-content: space-between;'>
				<div style='padding-left:12px' @tap="gotoPlan">
				<image style='width:35px; height:35px;' src='../../static/exchange.png'>
					<p>更换套餐</p>
					</div>
					<div style='padding-left:12px'>
				<image style='width:35px; height:35px' src='../../static/close.png'>
					<p>关闭帐户</p>
					</div>
					<div style='padding-left:12px'>
				<image style='width:35px; height:35px' src='../../static/contact.png'>
					<p>联系我们</p>
					</div>
			</div>
		</div>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				info:{},
				address:'40, 10 Park Row, RI, 403992',
				isPayed:''
			}
		},
		onLoad(options){
			var _this =this
			console.log(11)
			if(options.info){
			this.info = JSON.parse(options.info)
			}
			my.setStorage({
			  key: 'info',
			  data: _this.info,
			  success: function() {
			    my.alert({content: '写入成功'});
			  }
			})
			
			my.getStorage({
				key: 'info',
				  success: function(res) {
				    _this.info = res.data
				  },
				  fail:function(){
				}
			})
			console.log('info',this.info)
			my.getStorage({
				key: 'isPayed',
				  success: function(res) {
				    _this.isPayed = res.data
					console.log(res)
					
				  }
			})
			
			
			
				 
			
			
			
		},
		mounted(){
			console.log('ispay?',this.isPayed)
		},
		methods: {
			async payment(){
				uni.navigateTo({

				    url:"../paymentInfo/paymentInfo"
				})
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
			
			gotoPlan(){
			        uni.navigateTo({
			            // url: 'test?id=1&name=uniapp'  c传递参数
			            url:"../plan/plan"
			        })
			}
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
				border-radius: unset;
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
		top:75%;
		height:20%;
		position:absolute;
		image{
			    position: relative;
			    left: 50%;
			    transform: translate(-50%,0);
		}
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
