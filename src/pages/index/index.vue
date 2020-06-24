<template>
  <view class="content">
    <!-- 登录图片 -->
    <div class="login_img">
      <div class="text">移动平台</div>
      <img src="../../static/卡通地图@2x.png" alt="" class="img_logo" />
    </div>
    <!-- 登录区域 -->
    <div class="login">
      <ul>
        <li class="flex ">
          <span class="text_user">账号</span>
          <input
            v-model="user"
            type="text"
            maxlength="16"
            class="input_text"
            placeholder="请输入易通达账号"
          />
        </li>
        <li>
          <div class="border"></div>
        </li>
        <li class="flex">
          <span class="text_user">密码</span>
          <input
            v-model="pwd"
            type="password"
            maxlength="16"
            class="input_text"
            placeholder="请输入易通达密码"
          />
        </li>
      </ul>
    </div>
    <!-- 登录按钮 -->
    <div class="btn_box">
      <van-button
        class="btn"
        block
        type="primary"
        open-type="getUserInfo"
        lang="zh_CN"
        @getuserinfo="bindgetuserinfo"
        >登录</van-button
      >
    </div>
    <div>{{ list }}</div>

    <img :src="list.avatarUrl" alt="" />
    <div>{{ list.avatarUrl }}</div>
  </view>
</template>

<script>
export default {
  data() {
    return {
      list: "", //用户数据列表
      pwd: "", //用户密码
      user: "", //用户名
      wx_id: "", //微信id
    };
  },
  onLoad() {
    let that = this;
    uni.checkSession({
      success(res) {
        if (res) {
          that.routeTo(); //用户有登录信息的话直接跳转
        }
      },
      fail() {
        // 如果没有的话重新登录
        uni.showToast({
          title: "请重新登录",
          icon: "none",
        });
      },
    });
  },
  methods: {
    routeTo() {
      //转跳方法
      uni.redirectTo({
        url: "/pages/home/index",
      });
    },
    bindgetuserinfo(data) {
      if (data.detail.errMsg == "getUserInfo:fail auth deny"){
				uni.showModal({
					title:"请登录",
					success(){	
					}
				})
			}
        //获取用户信息方法
      if (data.detail.rawData) {
				// this.routeTo()
				console.log(data);
				
        this.list = JSON.parse(data.detail.rawData);
      }
    },
    login_first() {
      //初次登录绑定方法
      uni.request({
        url: "192.168.1.40/wxapp/user/first_login",
        data: {
          pwd: this.pwd,
          user: this.user,
        },
        success(res) {
          if (res.status == 200) {
            uni.showToast({
              title: "绑定成功",
              success() {
                this.routeTo();
              },
            });
          }
        },
      });
    },
    login() {
      //登录方法
      uni.request({
        url: "/user/login",
        data: {
          wx_id: this.wx_id,
        },
        success(res) {
          if (res.data.bind_status == 0) {
            this.login_first();
          } else {
            routeTo();
          }
        },
      });
    },
  },
};
</script>

<style lang="less">
.content {
  font-family: Arial;
  text-align: center;
  width: 100%;
  height: 100%;
  .login_img {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    align-items: center;
    width: 750rpx;
    height: 589rpx;
    background: linear-gradient(
      180deg,
      rgba(0, 93, 199, 1) 0%,
      rgba(43, 138, 212, 1) 100%
    );
    opacity: 1;
    display: flex;
    justify-content: center;
    align-items: center;
    .text {
      flex: 100%;
      font-size: 40rpx;
      color: rgba(255, 255, 255, 1);
    }
    .img_logo {
      width: 473rpx;
      height: 308rpx;
    }
  }
  .login {
    margin-top: 50rpx;
    width: 750rpx;
    height: 176rpx;
    font-size: 34rpx;
    line-height: 34rpx;
    .border {
      width: 720px;
      height: 1px;
      background: rgba(229, 229, 229, 1);
      opacity: 1;
      transform: translate(100rpx, 0);
    }
  }
  ul {
    .flex {
      display: flex;
      justify-content: center;
      align-items: center;
      .text_user {
        padding: 0 112rpx 0 30rpx;
        width: 70rpx;
        height: 48rpx;
        line-height: 48rpx;
      }
      .input_text {
        border: 1px solid #fff;
        background-color: #fff;
        height: 88rpx;
        width: 540rpx;
        text-align: left;
      }
    }
  }
  .btn_box {
    margin-top: 30rpx;
    width: 670rpx;
    height: 94rpx;
    margin: 40rpx auto;
    .btn {
      background: rgba(26, 173, 25, 1);
      width: 100%;
      height: 100%;
      color: aliceblue;
    }
  }
}
</style>
