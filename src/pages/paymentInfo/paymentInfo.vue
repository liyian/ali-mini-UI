<template>
	<view class='pay-info'>
		
		<h1>支付信息 Payment Info</h1>
		<div style='display: flex;align-items: center;'>
			<div style='width: 60%;'>
			<h2>房屋信息 Housing Info</h2>
			<p>Name: Yian Li</p>
			<p>Postal: 004319</p>
			<p>Address: 10 Park Row W,RI</p>
			</div>
			<image style='width: 40%; height:63px; ' src='../../static/house.png'></image>
		</div>
		
		<div>
		<h2>套餐信息 Tarrif Plan Info</h2>
		<p>Plan A</p>
		<p>Price: 92</p>
		<p>Subscription: 2020.12.20-2021.1.20</p>
		</div>
		
		<div>
		<h3>Total（EUR）<span> ¥92</span></h3>
		</div>
		
		<button style='width:100%;background:#0A1C53;color:white;'@tap='payment'>Confirm and Pay</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			async payment(){
				var _this = this
				var code = await this.getAuthCode()
				
				my.request({
				  url: 'https://edflabschina.cn:5758/aliPay/oauth',
				  method: 'POST',
				  data: {//data里的key、value是开发者自定义的
				    code: _this.authCode
				  },
				  dataType: 'json',
				  success: function(res) {
					console.log(res.data.order)
					my.tradePay({
					        tradeNO: res.data.order.trade_no,  
					        success: function(res) { 
								my.setStorage({
									key: 'isPayed',
									data:'payed',
									  success: function() { 
										  console.log('ok')
									  }
								})
								 uni.navigateTo({
									 url:"../user/user"				         			
								 })
					        },
					        fail: function(res) {  
					        
					       },
					});
				    
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

<style>
h1{
		color:#DA3E00;
		font-size:54rpx;
		font-weight: bold;
	}
	h2{
		color:#0A1C53;
		font-size:16px;
		font-weight: bold;
		margin-top:30px;
		
	}
	h3{
		font-size: 18px;
		padding: 20px;
		
	}
	span{
		font-size:27px;
		font-weight: bold;
		float: right;
	}
	.pay-info{
		margin:30px;
		line-height: 30px;
	}
	div{
		
		border-top:0.5px solid #D8D8D8;
	}
</style>
