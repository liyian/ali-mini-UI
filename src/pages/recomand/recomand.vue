<template>
	<view class='recomand'>
		<h1>为您推荐(Recommanded)</h1>
		<u-radio-group v-model="value">
			<div class='recomand-card' v-for="(item, index) in list" :key="index" >
					<div style='height: 40%; border-bottom:0.5px solid white'>
						<h2>标准计划（可调整）</h2>
						<u-radio 
							@change="radioChange" 
							:name="item.name"	
						>
						</u-radio>
					</div>
					
					<div style='display: flex;'>
						<div style='width: 50%;'>
							<p>结束日期：不固定</p>
							<p>结束费用：无</p>
							<p>每年节省金额：N/A</p>
							<p>每月个人预测：¥92</p>
						</div>
						
						<div style='width: 50%;display: flex;flex-direction: column;align-items: center;'><h3>¥ 92/月</h3>
						<u-button  type="info">选择此计划</u-button>
						</div>
					</div>
					</div>
				</u-radio-group>
			
			
		<div style='position:fixed;bottom:0;width:100%;display: flex;float:bottom'>
		<u-button @click="gotoHome" class='home' >返回主页</u-button>
		<u-button @click="gotoRecommand" type='primary' :custom-style='next'>下一页</u-button>
		</div>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list:[
					{name:'adjust'},
					{name:'fixed'}
				],
				value:'adjust'
			}
		},
		methods: {
			gotoRecommand(){
				uni.navigateTo({
				  
				    url:"../planMade/planMade"
							
				})
			},
			gotoHome(){
				uni.navigateTo({
				  
				    url:"../user/user"
							
				})
			}
		}
	}
</script>

<style lang='scss'>
.recomand{
	.u-btn{
		width: 60%;
		font-size: bold;
		border-radius: 10px;
	}
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
	h1{
		float:left;
		color:#DA3E00;
		font-size:54rpx;
	}
	
	h2{
		font-size:18px;
		font-weight: bold;
	}
	h3{
		font-size:26px;
		font-weight: bold;
	}
	.u-radio-group{
		width: 100%;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	
	.u-radio{
		float:right !important;
		padding-top:10px;
	}
	.u-radio__icon-wrap{
		color:white;
		border: 1px solid black;
		background-color: white;
		width: 21px !important;
		height:21px !important;
	}
	.u-radio__icon-wrap--checked{
		color:white;
		border: 1px solid black;
		background-color: white !important;
		.uicon-checkbox-mark{
			color:black !important;
		}
	}
	
	.recomand-card{
		padding:14px;
		color:white;
		margin-top:40px;
		background-image: linear-gradient(to right,#0E297A,#2A7FD0);
		width: 80%;
		height:200px;
		border-radius: 20px;
		box-shadow: 5px 5px 5px #888888;
	}
	
	.recomand-card:nth-child(2){
		background:#F2F2F2;
		color:black;
	}
	
	}
}
</style>
