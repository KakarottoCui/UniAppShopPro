<template>
	<view>
		<view class="user-section">
			<image class="bg" src="/static/user-bg.jpg"></image>
			<text class="bg-upload-btn yticon icon-paizhao"></text>
			<view class="portrait-box">
				<image class="portrait" :src="getHead()"></image>
				<text class="pt-upload-btn yticon icon-paizhao"></text>
			</view>
		</view>
		<view class="history-section icon u-p-20" v-if="user">
			<view class="sec-header u-tips-color">
				<text>个人信息</text>
			</view>
			<u-field
						v-model="user.name"
						label="用户名"
						placeholder="请填写用户名"
					>
					</u-field>
					<u-field
						v-model="user.phone"
						label="电话"
						placeholder="请填写用户电话"
					>
					</u-field>
					<u-field
						v-model="user.address"
						label="地址"
						type="textarea"
						placeholder="请填写地址"
					>
					</u-field>
					<u-button type="primary" @click="submit">修改信息</u-button>
		</view>
	</view>
</template>

<script>
	import appRequest from "@/common/appRequestUrl.js";
	import {  
	    mapState,  
	    mapMutations  
	} from 'vuex';  
	export default {
		data() {
			return {
				user:""
			};
		},
		computed:{
			...mapState(['userInfo']),
		},onShow() {
			this.user = appRequest.getUserInfo();
		},methods:{
			getHead:function(){
				switch (this.user.type){
					case 1:
						return '/static/user.jpg';
						break;
					case 2:
						return '/static/store.png';
						break;
					case 3:
						return '/static/admin.png';
						break;		
					default:
						return '/static/missing-face.png';
						break;
				}
			},submit:function(){
				if(!this.user.name||!this.user.phone||!this.user.address){
					uni.showToast({
						title:"用户名，电话，地址均需填写完整。",
						icon:"none"
					})
					return;
				}
				
				let _this = this;
				let dataStr = JSON.stringify({
							uid:_this.user.uid,
							phone:_this.user.phone,
							address:_this.user.address,
							name:_this.user.name,
						})
				appRequest.request({
					method: "POST",
					url: appRequest.changeInfo,
					data:{
						data:dataStr
					},
					success: function(res) {
						if(res.data.code == 200){
							_this.$api.msg(res.data.msg);
						}else{
							_this.$api.msg(res.data.msg);
						}
					},
					fail: function(res) {
						_this.$api.msg("网络异常");
					}
				})
				
			}
		}
	}
</script>

<style lang="scss">
	page{
		background: $page-color-base;
	}
	.user-section{
		display:flex;
		align-items:center;
		justify-content: center;
		height: 460upx;
		padding: 40upx 30upx 0;
		position:relative;
		.bg{
			position:absolute;
			left: 0;
			top: 0;
			width: 100%;
			height: 100%;
			filter: blur(1px);
			opacity: .7;
		}
		.portrait-box{
			width: 200upx;
			height: 200upx;
			border:6upx solid #fff;
			border-radius: 50%;
			position:relative;
			z-index: 2;
		}
		.portrait{
			position: relative;
			width: 100%;
			height: 100%;
			border-radius: 50%;
		}
		.yticon{
			position:absolute;
			line-height: 1;
			z-index: 5;
			font-size: 48upx;
			color: #fff;
			padding: 4upx 6upx;
			border-radius: 6upx;
			background: rgba(0,0,0,.4);
		}
		.pt-upload-btn{
			right: 0;
			bottom: 10upx;
		}
		.bg-upload-btn{
			right: 20upx;
			bottom: 16upx;
		}
	}


</style>
