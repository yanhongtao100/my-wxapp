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
    return {
      code: "",
    };
  },
  created() {
    var _this = this;
    uni.login({
      success(res) {
				_this.code = res.code;
      },
      fail() {
        uni.showToast({
          title: "网络连接失败，请稍后",
          icon: "none",
        });
      },
    });

    uni.checkSession({
      success(res) {
        if (res) {
          // console.log(res);
        }
      },
      fail() {
        // 如果没有的话重新登录
        uni.login({
          success(res) {},
        });
      },
    });
    //微信的校验权限事件
    wx.getSetting({
      success(res) {
        wx.getUserInfo({
          success(esr) {
            // console.log(esr, 666);
          },
        });
      },
    });
  },
  mounted() {},
  activited() {},
  update() {},
  beforeRouteUpdate() {},
  methods: {
	
    bindgetuserinfo(data) {
				let _this=this
      //获取用户信息方法
      if (data.detail.errMsg == "getUserInfo:fail auth deny") {
        //如果用户取消登录了
        uni.showModal({
          title: "请登录",
          success() {},
        });
      }
      if (data.detail.rawData) {
				uni.showLoading({
					title:'登陆中',
					mask:true
				})
        // 如果用户确认登录了
        try {
          // 尝试调登录接口
          uni.request({
            url: "http://localhost:8080/wxapp/user/login",
            data: {
              wx_id: this.code,
            },
            success(res) {
              // 如果绑定状态为1
              if (res.data.data.bind_status === 1) {
                // 如果登录状态为1？？？？？？？？？？？
                if (res.data.data.login_status === 1) {
                  uni.showToast({
                    title: "登录成功",
                    success() {
                      // 轻提示显示之后2秒开始跳转主页，最好带上id和身份信息？？？？？
                      setTimeout(function() {
                        uni.switchTab({
                          url:`/pages/home/index`,
                        });
                      }, 2000);
										},
										fail(rej){
											console.log(rej);
										}
                  });
                } else if (res.data.data.login_status === 0) {
                  // 如果登录状态为0，让用户稍后再试
                  uni.showModal({
                    title: "登录失败，请稍后重试",
                    showCancel: false,
                  });
                }
              } else if (res.data.data.bind_status === 0) {
                // 如果没绑定，就去让用户绑定
                uni.showModal({
                  title: "您还未绑定，请前去绑定",
                  showCancel: false,
                  success() {
                    uni.redirectTo({
                      url: "/pages/index/index",
                    });
                  },
                });
              }
            },
            fail(rej) {					
							// 如果出现问题，抛出问题
              uni.showToast({
                title: `服务器错误${rej.errMsg}`,
                icon: "none",
              });
            },
          });
        } catch (error) {
					throw error
				}
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
