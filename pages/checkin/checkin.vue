<template>
	<view>
		<camera device-position="front" flash="off" class="camera" @error="error" v-if="showCamera"></camera>
		<image mode="widthFix" class="image" :src="photoPath" v-if="showImage"></image>
		<view class="operate-container">
			<button type="primary" class="btn" @tap="clickBtn" :disabled="canCheckin">{{btnText}}</button>
			<button type="warn" class="btn" @tap="afresh" :disabled="canCheckin">重拍</button>
		</view>
		<view class="notice-container">
			<text class="notice">注意事项</text>
			<text class="desc">拍照签到的时候，必须要拍摄自己的正面照片，侧面照片会导致无法识别。另外，拍照的时候不要戴墨镜或者帽子，避免影响拍照签到的准确度。</text>
			</view>

		</view>
	</view>
</template>

<script>
	// var QQMapWX = require('../../lib/qqmap-wx-jssdk.min.js');
	// var qqmapsdk;
	export default {
		data() {
			return {
				canCheckin: false,
				photoPath: '',
				btnText: '拍照',
				showCamera: true,
				showImage: false
			}
		},
		// onLoad:function(){
		// 	qqmapsdk = new QQMapWX({
		// 		key:""
		// 	})
		// },
		methods: {
			clickBtn:function(){
				let that = this;
				if(that.btnText == "拍照"){
					let ctx = uni.createCameraContext();
					ctx.takePhoto({
						quality:'high',
						success:function(resp){
							console.log(resp.tempImagePath)
							that.photoPath = resp.tempImagePath
							that.showCamera=false
							that.showImage=true
							that.btnText = "签到"
						}	
					})
				}else{
					uni.showLoading({
						title:"签到中请稍后"
					})
					setTimeout(function(){
						uni.hideLoading()
					},30000)
					
					wx.getLocation({
					    type: 'wgs84',
					    success: function (res) {
					        console.log('当前位置的经度：' + res.longitude);
					        console.log('当前位置的纬度：' + res.latitude);
					    }
					});

					
					// uni.getLocation({
					// 	type:"wgs84",
					// 	success: function(resp){
					// 		let latitude = resp.latitude
					// 		let longitude = resp.longitude
					// 		console.log(latitude)
					// 		console.log(longitude)
							
					// 		qqmapsdk.reverseGeocoder({
					// 			location:{
					// 				latitude:latitude,
					// 				longitude:longitude
					// 			},
					// 			success:function(resp){
					// 				console.log(resp.result)
					// 				let address = resp.result.address
					// 				let addressComponent = resp.result.address_component
									
					// 			}
					// 		})
					// 	}
					// })
				}
			},
			afresh:function(){
				let that = this;
				that.showCamera = true;
				that.showImage = false;
				that.btnText = "拍照";
			}
		}
	}
</script>

<style lang="less">
	@import url("checkin.less");
</style>
