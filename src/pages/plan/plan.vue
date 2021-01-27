<template>
	<view class='plan'>
		<h1>房屋信息（1/2）</h1>
		
		<h2>1.缴费项目</h2>
		<u-radio-group v-model="value" >
			<u-radio 
				@change="radioChange" 
				v-for="(item, index) in list" :key="index" 
				:name="item.name"
				:disabled="item.disabled"
				shape="circle"
				active-color="#0f2057"
			>
				{{item.name}}
			</u-radio>
		</u-radio-group>
		
		<h2>2.房屋信息</h2>
				<div style='display: flex;'>
				<div class='card'>
				<image src='../../static/9.png'></image>
				</div>
						
				<div class='card'>
				<image src='../../static/8.png'></image>
				</div>
						
				<div class='card selected' >
				<image src='../../static/13.png'></image>
				</div>
							</div>			
		
		<h2>3.是否配备Econmy 7 Meter?</h2>
		<u-radio-group v-model="value2" >
			<u-radio 
				@change="radioChange" 
				v-for="(item, index) in list2" :key="index" 
				:name="item.name"
				:disabled="item.disabled"
				shape="circle"
				active-color="#0f2057"
			>
				{{item.name}}
			</u-radio>
		</u-radio-group>
		
		<h2>4.是否配备Smart Meter?</h2>
		<u-radio-group v-model="value2" >
			<u-radio 
				@change="radioChange" 
				v-for="(item, index) in list2" :key="index" 
				:name="item.name"
				:disabled="item.disabled"
				shape="circle"
				active-color="#0f2057"
			>
				{{item.name}}
			</u-radio>
		</u-radio-group>
		
		<h2>5.是否配备Credit Meter Prepayment?</h2>
		<u-radio-group v-model="value2" >
			<u-radio 
				@change="radioChange" 
				v-for="(item, index) in list2" :key="index" 
				:name="item.name"
				:disabled="item.disabled"
				shape="circle"
				active-color="#0f2057"
			>
				{{item.name}}
			</u-radio>
		</u-radio-group>
		
		<div style='width:100%; display: flex;'>
		<u-button @click="gotoHome" class='home' >返回主页</u-button>
		<u-button @click="gotoRecommand" type='primary' :custom-style='next'>下一页</u-button>
		</div>
	</view>
</template>

<script>
	next:{
		color:'white';
		background:'blue';
	}
	export default {
		data() {
			return {
				list: [
					{
						name: '电(electric)',
						disabled: false
					},
					{
						name: '气(gas)',
						disabled: false
					},
					{
						name: '电和气(both)',
						disabled: false
					}
				],
				list2:[
					{name: '是(Yes)',
						disabled: false},
						{name: '否(No)',
							disabled: false},
							{name: '不清楚(Undecided)',
								disabled: false},
				],
				// u-radio-group的v-model绑定的值如果设置为某个radio的name，就会被默认选中
				value: '电(electric)',
				value2: '是(Yes)',
			}
		},
		methods: {
			
			// 选中某个单选框时，由radio时触发
					radioChange(e) {
						console.log(e,'change');
					},
					// 选中任一radio时，由radio-group触发
					radioGroupChange(e) {
						 console.log(e);
					},
					gotoRecommand(){
						uni.navigateTo({
						  
						    url:"../recomand/recomand"
									
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
p{
	
}
.plan{
	.u-btn{
		width: 50%;
		font-size: bold;
	}
	h1{
		color:#DA3E00;
		font-size:54rpx;
	}
	h2{
		color:#0A1C53;
		font-size:32rpx;
		font-weight: bold;
		margin-top: 50px;
		padding: 10px;
	}
}
.u-radio-group{
	padding: 10px;
}
.card{
	box-shadow: 5px 5px 5px #888888;
	
	width:30%;
	height: 100px;
	image{
		width: 80%;
		height: 80%;
		position: relative;
		top:50%;
		left:50%;
		transform:translate(-50%,-50%);
		
	}
}
.selected{
	background-color: #0f2057;
}
</style>
