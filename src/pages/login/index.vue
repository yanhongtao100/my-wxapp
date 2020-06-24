<template>
  <div class="container">
    <img src="../../static/掌上e监测@2x.png" class="img_text" alt="" />
    <img src="../../static/卡通地图@2x.png" class="img_bg" alt="" />
    <div class="btn_box">
      <van-button
        class="btn"
        block
        open-type="getUserInfo"
        lang="zh_CN"
        @getuserinfo="bindgetuserinfo"
        >登录</van-button
      >
    </div>
  </div>
</template>

<script>
export default {
  name: "",
  components: {},
  props: {},
  data() {
    return {};
  },
  created() {
    uni.checkSession({
      success(res) {
        if (res) {
				 console.log(res);
				 
        }
      },
      fail() {
        // 如果没有的话重新登录
        uni.login({
					success(res){
						console.log(res);
						
					}
				})
      },
		});
		//微信的校验权限事件
		wx.getSetting({
			success(res){
				wx.getUserInfo({
					success(esr){
						console.log(esr,666);
					}
				})
				
			}
		})
  },
  mounted() {},
  activited() {},
  update() {},
  beforeRouteUpdate() {},
  methods: {
    bindgetuserinfo(data) {
      //获取用户信息方法
      if (data.detail.errMsg == "getUserInfo:fail auth deny") {
        console.log(data);

        uni.showModal({
          title: "请登录",
          success() {},
        });
      }
      if (data.detail.rawData) {
        console.log(data);
        uni.request({
          url: "http://192.168.1.40:8080/wxapp/user/login",
          data: {
            w_id: 1000,
          },
          success(res) {
            console.log(res.data.data);
          },
          fail() {
            console.log(222);
          },
        });
        // 转跳方法
        // uni.redirectTo({
        //   url: "/pages/home/index",
        // });
      }
    },
  },
  filter: {},
  computed: {},
  watch: {},
};
</script>

<style lang="less">
.container {
  position: absolute;
  height: 100vh;
  width: 750rpx;
  background: linear-gradient(
    180deg,
    rgba(0, 93, 199, 1) 0%,
    rgba(43, 138, 212, 1) 100%
  );
  .img_text {
    width: 576rpx;
    height: 271rpx;
    position: absolute;
    top: 82rpx;
    left: 50%;
    transform: translate(-50%, 0);
  }
  .img_bg {
    width: 473rpx;
    height: 308rpx;
    position: absolute;
    top: 447rpx;
    left: 50%;
    transform: translate(-50%, 0);
  }
  .btn_box {
    position: absolute;
    bottom: 186rpx;
    left: 50%;
    transform: translate(-50%, 0);
    margin-top: 30rpx;
    width: 670rpx;
    height: 94rpx;
    margin: 40rpx auto;
    color: rgba(72, 138, 225, 1);
    .btn {
      background: #fff;
      font-size: 36rpx;
      font-family: PingFang SC;
      font-weight: 400;
      line-height: 50rpx;
      color: rgba(72, 138, 225, 1);
      opacity: 1;
    }
    .van-button__text {
      color: rgba(72, 138, 225, 1);
    }
  }
}
</style>
