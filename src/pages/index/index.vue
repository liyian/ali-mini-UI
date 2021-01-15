<template>
	<view class="content">
		<image class="logo" src="/static/logo.png"></image>
		<view>
			<button @getAuthorize="onGetAuthorize" scope="userInfo" open-type="getAuthorize" class="login">用户信息授权</button>
			<button @getAuthorize="getAlPhoneNumber" scope="phoneNumber" open-type="getAuthorize" class="login">手机号授权登录</button>
		
			<button type="primary"  @click="getAuthCode">
			      获取授权码
			    </button>	
					
					<button type="primary"  @click="payment">
					      付款
					    </button>
		</view>
	</view>
</template>

<script>
	const {KJUR, hextob64} = require('jsrsasign')
	const PEM_BEGIN = '-----BEGIN PRIVATE KEY-----\n'
	const PEM_END = '\n-----END PRIVATE KEY-----'

	export default {
		data() {
			return {
				authCode:'',
				privateKey:'MIIEvQIBADANBgkqhkiG9w0BAQEFAASCBKcwggSjAgEAAoIBAQCMOOUw/3xN209UCSysLyD2wicr/957Pwt/qAHsX1V8EUWyy30ksdUqMw9wy5tP/65vOmfaDq7RPg56x9BlhGeEB98BnYIOsarvSZ8XEk2XGKiR4DDau7Rqqt1t6+PXjaRY7IFXtStEwGj0i9pdAo3b71DkEum4uKhSUUuylm8pi1mCQ1mUioNP2T/aIDo0dcOTefRKaBvpQBPKCrDgJ6LNhI5IgyL7522pjf0czpbWzHiMxYqSxxShOaU5IriJLiQUC2llsq4rDVnTdCLW9GZtyMDBXtUvx6jRq7fPAZ8g5VPPguHctVt/k+iVoNkPr79vP8+V2omv2zxB3ITxEka7AgMBAAECggEAR2TQjNoIGDwJb+gb7q8wQXMpLqXgSTAYUq7gC9GeXMUOj7I7zWsd9ZZ5zhxcv6aW0dsS9Vt0duT5ylniKU34xqibSt8/UOv09FCvx7dx+fR8H4neVgAsJANLKqRoJV/gxcXlNRrxJNgymBB85c5fHBQFQ5q9lWevi/oIaK1fe+7OLxaECGbJd0FwMCwBQ94iaTrgTEANz3VwzosXZ6YGOV7CBXXUel++2Fb9LzKGbyASEqUCVCwgbVhcBMHhfKmDWt1a4sjrV4t4hGAQ8B2ab8gsMv+KnlvQnHe59MeL/9KQE7P3Z2CG0mDMyB23Yhefk2+t2r8wflddq6zlEVcXgQKBgQDIaTwH7s5olLUvXbFAgcJPW44RJtYAcCaOFEyB/cUJlWn+2UpJNYxqqSpHZgo4zejyVzZie1ppt+uTip5KsvLuMrkHDjaznbrxXsIo6cFU59I3nKA9VtImMQ9gtSUMwz7R69IWrpS1t/3NI2LU5sTaAt9b16oPJqiFJOMx9xzUgwKBgQCzHcel3SLFvXNtlNQ8K18Dea0WdMTJyPyM+66MRx+H2rZeTRkatZFCR5o7GSfA5ilMvYOz1gJj40i4CILJDZHP5pg/WzL0mFE9zPMd8/ZJVZQegkDKUJfbrpoU13DpdqQ8u1XfKcU1/cjPdJ3jwTMPz5D9ovYGEBqgAy0esovfaQKBgETq82ri83ZBVUFRnefJ95EksekyCruh1ZBcaKb3JrASAp5SCNq+IGh5fgGJlJBONwJHfXtGYiQizE8zhxW3DhVdugU0HRmNb0EbzfcV05coFUFsiiot1hyuGFSwVmpaJhs4VknYhWksOYo21gDSzvUNsdO6iU+mgqMuuTZwfSiTAoGBAKqFV2SYuln2msVgQw1lXPc6B4axdAeGyKiPkwUJUQNNPQFZ9ORwpvGqqTJKKIaEmBzu70tuGqht4PUTZS9zM7Xxmk/2yvQBbpafX3TMpVP7nR/OcVAqmegSua3NzTj2MjV/Q6lJYu6bxWl7ZDVH4QzUVvyhAAeVnUNowrPO10rBAoGAKZHbwq32hmMXybPFnNncLPqQvTH1gjZVYfAuZo50/8KynVclWdjRKLBtYVTQzqccKziajE4ISD5XIMxSaReif/WolmjB3+H33yR0oelZOxs5V7cMdR5u1wsKsPwNcZrTj7b2pIZeTuhU5hlMz2HzXsWo/lJvSVX1Jl6TPI/y2EM='
			}
		},
		onLoad() {

		},
		methods: {
			payment(){
				my.tradePay({
						tradeNO: '2017111521001104105336677922',  
						success: function(res) {            
						  my.alert({
						   content: JSON.stringify(res),
						 });
				
						},
						fail: function(res) {  
						my.alert({
						  content: JSON.stringify(res),
						  });
				
					   },
			    });
			},
			rsaSign(content,privateKey,hash='SHA256withRSA'){
				 privateKey = this._formatKey(privateKey)
				
				 const signature = new KJUR.crypto.Signature({
				            alg: hash,
				            //!这里指定 私钥 pem!
				            prvkeypem: privateKey
				        })
				        signature.updateString(content)
				        const signData = signature.sign()
				        // 将内容转成base64
				        return hextob64(signData)
			},
			_formatKey(key) {
			        if (!key.startsWith(PEM_BEGIN)) {
			            key = PEM_BEGIN + key
			        }
			        if (!key.endsWith(PEM_END)) {
			            key = key + PEM_END
			        }
			        return key
			    },
			getUserId(){
				var _this = this
				console.log(this.authCode)
				var wait_sign = "app_id=2021002119669650&charset=GBK&code="+this.authCode.toString()+"&grant_type=authorization_code&method=alipay.system.oauth.token&sign_type=RSA2&timestamp:2020-12-02 14:46:50&version=1.0"
				var sign  = this.rsaSign(wait_sign,this.privateKey)
				console.log(wait_sign)
				console.log(sign)
				uni.request({
					url:'https://openapi.alipay.com/gateway.do',
					method:'POST',
					header: {
						'content-type': 'application/x-www-form-urlencoded', 
					},
					data:{
						code:_this.authCode,
						app_id:'2021002119669650',
						method:'alipay.system.oauth.token',
						charset:'GBK',
						sign_type:'RSA2',
						sign:sign,
						timestamp:'2020-12-02 14:46:50',
						version:'1.0',
						grant_type:'authorization_code'
						
					},
					success: res=>{
						console.log(res)
					},
					fail: res => {
						console.log(res)
						uni.showToast({

							title: '错误: '+res.errMsg
						})
					}
				})
			},
			async getAuthCode(){
				var _this = this
				await my.getAuthCode({
				  scopes: 'auth_user',
				  success: (res) => {
					_this.authCode =res.authCode
					uni.showToast({
						title:'登陆中...'
					})
					
				  },
				})
				
				await this.getUserId()
			},
			getAlPhoneNumber() {
				my.getPhoneNumber({
					scopes: "auth_user",
					success: res => {
						var resData = JSON.parse(res.response);
						console.log('res',resData)
						
					},
					fail: res => {
						console.log(res)
						uni.showToast({
							
							title: '错误: '+res.errorMessage
						})
					}
				});
			},
			onGetAuthorize(res) {
			    my.getOpenUserInfo({
			        fail: (res) => {},
			        success: (res) => {
			            let userInfo = JSON.parse(res.response).response // 以下方的报文格式解析两层 response
			            console.log(userInfo);
			            my.alert({
			                content: userInfo
			            });
			        },
			    });
			},
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin: 200rpx auto 50rpx auto;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>
