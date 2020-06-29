<template>
  <div>
    <div class="container">
      <van-nav-bar
        title="标题"
        left-text="返回"
        left-arrow
        @click-left="onClickLeft"
      />
      <div class="header_box">
        <div class="title_text">
          {{ orderMessage.title }}
        </div>
        <div class="time">
          {{ orderMessage.warning_time }}
        </div>
      </div>
      <div class="particulars">
        <van-cell-group>
          <van-cell title="警报描述:" :value="orderMessage.equipment" />
          <van-cell title="警报地区:" :value="orderMessage.proxi" />
          <van-cell title="警报设备:" :value="orderMessage.equipment" />
          <van-cell title="设备IP:" :value="orderMessage.equipmen_id" />
          <van-cell title="设备编号:" :value="orderMessage.equipmen_serial" />
        </van-cell-group>
      </div>
      <div v-if="orderMessage.is_look">
        <h2>未派遣负责人</h2>
        <van-button type="primary">指定负责人</van-button>
      </div>
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
      orderMessage: {},
    };
  },
  created() {},
  onLoad(e) {
    var _this = this;
    uni.request({
      url: `http://localhost:8080/wxapp/order/info`,
      data: {
        id: e.id,
      },
      success(re) {
        _this.orderMessage = re.data.data;
        console.log(_this.orderMessage);
      },
    });
  },
  mounted() {},
  activited() {},
  update() {},
  beforeRouteUpdate() {},
  methods: {
    onClickLeft() {

      uni.switchTab({
        url: "/pages/home/index",
        success(res) {
          console.log(res);
        },
        fail(rej) {
          console.log(rej);
        },
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
  background: rgba(239, 239, 244, 1);
  min-height: 100vh;
  .header_box {
    width: 750rpx;
    height: 206rpx;
    background-color: #fff;
    text-align: center;
    overflow: hidden;
    .title_text {
      margin: 35rpx auto;
      height: 46rpx;
      font-size: 40rpx;
      font-family: PingFang SC;
      font-weight: bold;
      line-height: 56rpx;
      color: rgba(51, 51, 51, 1);
      opacity: 1;
    }
    .time {
      font-size: 36rpx;
      font-family: PingFang SC;
      font-weight: 400;
      line-height: 50rpx;
      color: rgb(53, 53, 53);
      opacity: 1;
    }
  }
  .particulars {
    width: 750rpx;
    height: 405rpx;
    background: rgba(255, 255, 255, 1);
    margin-top: 20rpx;
  }
  h2 {
    margin: 50rpx 0;
    text-align: center;
    font-size: 32rpx;
    font-family: PingFang SC;
    font-weight: 400;
    line-height: 45rpx;
    color: rgba(86, 123, 149, 1);
    opacity: 1;
  }
}
.van-cell__title {
  flex: 21% !important;
  font-size: 32rpx !important;
  font-family: PingFang SC !important;
  font-weight: 400 !important;
  line-height: 45rpx !important;
  color: rgba(136, 136, 136, 1) !important;
  opacity: 1 !important;
  text-align: right !important;
}
.van-cell__value {
  flex: 79% !important;
  text-align: left !important;
  font-size: 32rpx !important;
  font-family: PingFang SC !important;
  font-weight: bold !important;
  line-height: 45rpx !important;
  color: rgba(53, 53, 53, 1) !important;
  opacity: 1 !important;
  padding-left: 40rpx;
}
.van-button--normal {
  width: 690rpx;
  font-size: 36rpx !important;
  margin: 30rpx;
}
</style>
