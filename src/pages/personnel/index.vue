<template>
  <div>
    <van-nav-bar title="维护小组" left-arrow fixed @click-left="goBack" />
    <div class="placeholder"></div>
    <div class="container">
      <van-swipe-cell right-width="65">
        <div class="card_container">
          <div class="img">
            <img :src="imageURL" alt="" />
          </div>
          <div class="text">
            <div class="name">张维护</div>
            <div class="phone">12312312312</div>
          </div>
        </div>
        <view slot="right" class="right">删除</view>
      </van-swipe-cell>
      <div class="btn">
        <van-button plain hairline type="danger" size="large"
          >细边框按钮</van-button
        >
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
      imageURL: "/static/blue.png",
    };
  },
  created() {},
  mounted() {},
  activited() {},
  update() {},
  beforeRouteUpdate() {},
  methods: {},
  filter: {},
  computed: {},
  watch: {},
};
</script>
<style lang="less">
.placeholder {
  width: 750rpx;
  height: 160rpx;
}
.container {
  background: rgba(239, 239, 244, 1);
  width: 750rpx;
  min-height: 100vh;
  .card_container {
    background: rgba(255, 255, 255, 1);
    width: 750rpx;
    height: 176rpx;
    font-size: 36rpx;
    font-family: Source Han Sans CN;
    font-weight: 400;
    color: rgba(0, 0, 0, 1);
    display: flex;
    justify-content: center;
    align-items: center;
    .img {
      margin-left: 30rpx;
      width: 128rpx;
      height: 128rpx;
      img {
        width: 100%;
        height: 100%;
      }
    }
    .text {
      margin-left: 30rpx;
      flex: 69.8%;
    }
  }
  .right {
    display: block;
    width: 200%;
    height: 100%;
    text-align: center;
    line-height: 176rpx;
    color: aliceblue;
    background-color: #f00;
  }
  .btn {
    padding: 0 20rpx;
  }
}
</style>
