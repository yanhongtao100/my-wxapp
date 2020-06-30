<template>
  <div class="container">
    <van-nav-bar title="我的信息" />
    <div class="bg">
      <div class="info">
        <div class="photo">
          <img :src="info.avatarUrl" alt="" />
        </div>
        <div class="name">{{ info.nickName }}</div>
        <van-tag type="success" round v-if="identity == 0">管理员</van-tag>
        <van-tag type="primary" round v-if="identity == 1">监测小组</van-tag>
        <van-tag type="warning" round v-if="identity == 2">维护小组</van-tag>
      </div>
    </div>
    <div class="txt_container">
      <van-cell-group title=" ">
        <van-cell
          title="历史警报"
          value=""
          is-link
          size="large"
          @click="goHistory"
        />
      </van-cell-group>
      <template v-if="identity != 2">
        <van-cell-group title=" ">
          <van-cell
            title="新建工单"
            value=""
            is-link
            size="large"
            @click="goCreate"
          />
        </van-cell-group>
        <van-cell-group title=" ">
          <van-cell
            title="检测小组"
            value="（6）"
            is-link
            size="large"
            @click="goPersonnel"
          />
        </van-cell-group>
        <van-cell-group title=" ">
          <van-cell title="维护小组" value="（6）" is-link size="large" />
        </van-cell-group>
      </template>
    </div>
    <van-button type="default" block class="btn">退出登录</van-button>
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
      identity: 0,
      info: {},
    };
  },
  created() {
    var _this = this;
    uni.login({
      success(res) {
        _this.code = res.code;
      },
    });
    uni.request({
      url: `http://localhost:8080/wxapp/user/info`,
      data: {
        code: _this.code,
      },
      success(res) {
        _this.identity = res.data.identity;
      },
    });
    uni.getUserInfo({
      success(res) {
        if (res.errMsg == "getUserInfo:ok") {
          _this.info = JSON.parse(res.rawData);
          console.log(_this.info);
        } else {
          uni.showToast({
            title: "获取用户身份失败，请稍后重试",
            icon: none,
          });
        }
      },
    });
  },
  mounted() {},
  activited() {},
  update() {},
  beforeRouteUpdate() {},
  methods: {
    goPersonnel() {
      uni.navigateTo({
        url: `/pages/personnel/index`,
      });
    },
    goCreate() {
      uni.navigateTo({
        url: `/pages/createOrder/index?code=${this.code}`,
      });
    },
    goHistory() {
      uni.navigateTo({
        url: `/pages/history/index?code=${this.code}`,
      });
    },
  },
  filter: {},
  computed: {},
  watch: {},
};
</script>

<style lang="less">
.container {
  width: 750rpx;
  height: 100vh;
  position: relative;
  background: rgba(239, 239, 244, 1);
  .bg {
    width: 750rpx;
    height: 252rpx;
    background: linear-gradient(
      180deg,
      rgba(0, 93, 199, 1) 0%,
      rgba(43, 138, 212, 1) 100%
    );
    opacity: 1;
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
    padding: 10rpx 0;

    .info {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      width: 161prx;
      height: 250rpx;

      .photo {
        margin: 15rpx;
        border-radius: 50%;
        width: 110rpx;
        height: 110rpx;
        img {
          border-radius: 50%;
          width: 100%;
          height: 100%;
        }
      }
      .name {
        margin: 5rpx;
        font-size: 40rpx;
        font-family: PingFang SC;
        font-weight: 400;
        line-height: 56rpx;
        color: rgba(255, 255, 255, 1);
        opacity: 1;
      }
      /deep/.van-tag {
        margin: 5rpx;
      }
    }
  }
  .txt_container {
    width: 750rpx;
    height: 100%-252rpx;
    /deep/.van-cell-group__title {
      padding-top: 8rpx;
    }
  }
  .btn {
    width: 700rpx;
    padding: 25rpx;
    position: absolute;
    bottom: 30rpx;
  }
}
</style>
