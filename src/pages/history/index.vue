<template>
  <div class="boy">
    <van-nav-bar
      title="标题"
      left-arrow
      fixed
      @click-left="goBack"
    />
    <div class="container">
      <div class="header">
        <div class="tab">
          <van-tabs swipeable animated :active="active">
            <van-tab title="一周">
              <div
                class="card"
                v-for="(item, index) in week_data"
                :key="index"
                @click="getCardMessage(item.id)"
              >
                <div class="title">{{ item.title }}</div>
                <div class="top">警报位置：{{ item.proxi }}</div>
                <div class="text">
                  <div class="left">警报时间：{{ item.time }}</div>
                  <div class="right">经手人：{{ item.personnel }}</div>
                </div>
              </div>
            </van-tab>
            <van-tab title="一个月">
              <div
                class="card"
                v-for="(item, index) in month_data"
                :key="index"
                @click="getCardMessage(item.id)"
              >
                <div class="title">{{ item.title }}</div>
                <div class="top">警报位置：{{ item.proxi }}</div>
                <div class="text">
                  <div class="left">警报时间：{{ item.time }}</div>
                  <div class="right">经手人：{{ item.personnel }}</div>
                </div>
              </div>
            </van-tab>
            <van-tab title="三个月">
              <div
                class="card"
                v-for="(item, index) in three_data"
                :key="index"
                @click="getCardMessage(item.id)"
              >
                <div class="title">{{ item.title }}</div>
                <div class="top">警报位置：{{ item.proxi }}</div>
                <div class="text">
                  <div class="left">警报时间：{{ item.time }}</div>
                  <div class="right">经手人：{{ item.personnel }}</div>
                </div>
              </div>
            </van-tab>
          </van-tabs>
        </div>
        <div class="select">
          负责人
        </div>
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
      active: 0,
      week_data: [],
      month_data: [],
      three_data: [],
    };
  },
  created() {},
  onLoad(e) {
    console.log(e);
    var _this = this;
    this.code = e.code;
    uni.request({
      url: `http://localhost:8080/wxapp/my/history`,
      data: {
        code: e.code,
      },
      success(res) {
        _this.week_data = res.data.week.data;
        _this.month_data = res.data.month.data;
        _this.three_data = res.data.three_month.data;
      },
    });
  },
  mounted() {},
  activited() {},
  update() {},
  beforeRouteUpdate() {},
  methods: {
    getCardMessage(id) {
      var _this = this;
      uni.navigateTo({
        url: `/pages/workOrderDetails/index?id=${id}&code=${_this.code}`,
      });
    },
    goBack() {
      uni.switchTab({
        url: "/pages/my/index",
        success(res) {
          console.log(res);
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
.boy {
  width: 750rpx;
  height: 100%;
  .container {
    margin-top: 150rpx;
    width: 750rpx;
    .header {
      position: relative;
      width: 750rpx;
      height: 100%;

      .tab {
        flex: 1;
        width: 750rpx;
        height: 100%;
        // background: rgba(239, 239, 244, 1);
        /deep/.van-tabs--line .van-tabs__wrap {
          height: 100rpx;
        }
        /deep/.van-tab {
          line-height: 100rpx;
          font-size: 36rpx;
          flex: 0.215;
          background: rgba(250, 250, 250, 1);
        }
        /deep/.van-tab__pane {
          min-height: 60vh;
        }
        /deep/.van-tabs__line {
          display: none;
        }
        /deep/.van-tab--active {
          color: rgba(0, 108, 191, 1);
        }
        /deep/.van-tabs--line {
          padding-top: 100rpx;
        }

        .card {
          margin-bottom: 20rpx;
          margin-top: 20rpx;
          margin-left: 15rpx;
          width: 680rpx;
          height: 214rpx;
          background: #fffffc;
          box-shadow: 0px 2rpx 8rpx rgba(53, 53, 53, 0.5);
          opacity: 1;
          padding: 20rpx;
          border-radius: 10rpx;
          display: flex;
          flex-direction: column;
          justify-content: space-around;
          align-items: flex-start;
          font-size: 36rpx;
          font-family: PingFang SC;
          font-weight: bold;
          line-height: 50rpx;
          color: rgba(68, 68, 68, 1);
          opacity: 1;
          .text {
            font-size: 28rpx;
            font-family: PingFang SC;
            font-weight: 400;
            line-height: 33rpx;
            color: rgba(68, 68, 68, 1);
            opacity: 1;
            width: 100%;
            display: flex;
            align-items: center;
            justify-content: space-around;

            .left {
              width: 68%;
            }
            .right {
              width: 31%;
            }
          }
          .top {
            font-size: 28rpx;
            font-family: PingFang SC;
            font-weight: 400;
            line-height: 33rpx;
            color: rgba(68, 68, 68, 1);
            opacity: 1;
          }
        }
      }
      .select {
        text-align: center;
        position: absolute;
        top: 0;
        right: 0;
        z-index: 2;
        width: 249rpx;
        height: 100rpx;
        font-size: 30rpx;
        font-family: PingFang SC;
        background: rgba(250, 250, 250, 1);
        font-weight: 400;
        line-height: 100rpx;
        border-left: 1px solid rgba(204, 204, 204, 1);
        opacity: 1;
      }
    }
  }
}
</style>
