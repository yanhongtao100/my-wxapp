<template>
  <div class="container">
    <!-- 头部 -->
    <div class="header">
      <div class="warning_text">
        <h3>警报通知</h3>
      </div>
      <div class="waring_grop">
        <div class="warning_box">
          <div
            class="num_box"
            :class="{ num_box_active: is_underway }"
            @click="getUnderway"
          >
            <h2>{{ info.underway }}</h2>
            <span>待处理</span>
          </div>
        </div>
        <div class="warning_box">
          <div
            class="num_box"
            :class="{ num_box_active: is_peading }"
            @click="getPeading"
          >
            <h2>{{ info.peading }}</h2>
            <span>进行中</span>
            <div class="border_left"></div>
            <div class="border_right"></div>
          </div>
        </div>
        <div class="warning_box">
          <div
            class="num_box"
            :class="{ num_box_active: is_finish }"
            @click="getFinish"
          >
            <h2>{{ info.finish }}</h2>
            <span>已完成</span>
          </div>
        </div>
      </div>
      <div class="work_order">
        <div class="order">
          <img src="/static/组 38@2x.png" alt="" />
          <span>警报工单</span>
        </div>
        <div class="order">
          <img src="/static/组 39@2x.png" alt="" />
          <span>人工工单</span>
        </div>
        <div class="border_order"></div>
      </div>
      <div class="portrait">
        <img :src="user.avatarUrl" alt="" class="avatar" />
      </div>
    </div>

    <div class="card_container">
      <div class="card" v-for="(item, index) in info.data" :key="index">
        <div class="order_title">
          {{ item.title }}
          <template v-if="item.event == 0">
            <van-tag round type="primary">已完成</van-tag>
          </template>
          <template v-if="item.event == 1">
            <van-tag round type="warning">待处理</van-tag>
          </template>
          <template v-if="item.event == 2">
            <van-tag round type="success">进行中</van-tag>
          </template>
        </div>
        <div class="order_container">
          <span class="warning_container_r">警报位置：{{ item.proxi }}</span>
          <span class="warning_container_r">警报时间：{{ item.time }}</span>
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
      code: "",
      info: {},
      user: {},
      is_underway: true,
      is_peading: false,
      is_finish: false,
    };
  },
  onLoad(e) {
    let _this = this;
    _this.code = e.code;
    uni.request({
      url: `http://localhost:8080/wxapp/user/info`,
      data: {
        code: e.code,
      },
      success(res) {
        console.log(res.data);
        // 如果是管理员
        if (res.data.identity === 0) {
          uni.request({
            url: `http://localhost:8080/wxapp/home/msg`,
            data: {
              code: e.code,
              event: 1,
            },
            success(re) {
              _this.info = re.data;
              console.log("管理员", _this.info);
            },
          });
        } else if (res.data.identity === 1) {
        } else if (res.data.identity === 2) {
        } else {
          uni.showToast({
            title: "参数错误",
            icon: "none",
          });
        }
      },
    });
  },
  created() {
    let _this = this;
    uni.getUserInfo({
      success(res) {
        _this.user = JSON.parse(res.rawData);
        console.log(_this.user);
      },
    });
  },
  mounted() {},
  activited() {},
  update() {},
  beforeRouteUpdate() {},
  methods: {
    getUnderway() {
      this.is_finish = false;
      this.is_peading = false;
      this.is_underway = true;
      var that = this;
      uni.request({
        url: `http://localhost:8080/wxapp/home/msg`,
        data: {
          code: that.code,
          event: 1,
        },
        success(re) {
          that.info = re.data;
          console.log("管理员", that.info);
        },
      });
    },
    getPeading() {
      this.is_finish = false;
      this.is_peading = true;
      this.is_underway = false;
      var that = this;
      uni.request({
        url: `http://localhost:8080/wxapp/home/msg`,
        data: {
          code: that.code,
          event: 2,
        },
        success(re) {
          that.info = re.data;
          console.log("管理员", that.info);
        },
      });
    },
    getFinish() {
      this.is_finish = true;
      this.is_peading = false;
      this.is_underway = false;
      var that = this;
      uni.request({
        url: `http://localhost:8080/wxapp/home/msg`,
        data: {
          code: that.code,
          event: 0,
        },
        success(re) {
          that.info = re.data;
          console.log("管理员", that.info);
        },
      });
    },

    onPullDownRefresh() {
      console.log(123);

      setTimeout(function() {
        uni.stopPullDownRefresh();
        console.log(123456);
      }, 50);
    },
  },
  filter: {},
  computed: {},
  watch: {},
};
</script>

<style lang="less" scoped>
.van-tag {
  font-size: 10rpx !important;
}
.container {
  width: 100%;
  height: 100%;
  .header {
    width: 750rpx;
    height: 550rpx;
    background: linear-gradient(
      180deg,
      rgba(0, 93, 199, 1) 0%,
      rgba(43, 138, 212, 1) 100%
    );
    opacity: 1;
    position: relative;
    .warning_text {
      text-align: center;
      height: 100 rpx;
      margin: 0rpx auto;
      padding-top: 193rpx;
      h3 {
        font-size: 70rpx;
        font-family: PingFang SC;
        font-weight: 400;
        line-height: 70rpx;
        color: rgba(255, 255, 255, 1);
        opacity: 1;
      }
    }
    .waring_grop {
      width: 750rpx;
      height: 156rpx;
      margin-top: 68rpx;
      display: flex;
      justify-content: center;
      align-items: center;
      .warning_box {
        width: 750rpx/3;
        height: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
        .num_box {
          width: 156rpx;
          height: 156rpx;

          position: relative;
          display: flex;
          justify-content: center;
          align-items: center;
          flex-wrap: wrap;
          h2 {
            flex: 100%;
            text-align: center;
            font-size: 80rpx;
            font-family: PingFang SC;
            font-weight: 400;
            line-height: 112rpx;
            color: rgba(255, 255, 255, 1);
            text-shadow: 0px 3px 6px rgba(0, 0, 0, 0.16);
            opacity: 1;
          }
          span {
            font-size: 22rpx;
            font-family: PingFang SC;
            font-weight: 400;
            line-height: 30rpx;
            color: rgba(255, 255, 255, 1);
            opacity: 1;
          }
          .border_left {
            position: absolute;
            top: 50%;
            left: -50rpx;
            transform: translate(0, -50%);
            width: 1rpx;
            height: 70rpx;
            background-color: rgba(255, 255, 255, 1);
          }
          .border_right {
            position: absolute;
            top: 50%;
            left: 200rpx;
            transform: translate(0, -50%);
            width: 1rpx;
            height: 70rpx;
            background-color: rgba(255, 255, 255, 1);
          }
        }
        .num_box_active {
          background: linear-gradient(
            180deg,
            rgba(255, 255, 255, 0.1) 0%,
            rgba(255, 255, 255, 0) 100%
          );
          opacity: 1;
          border-radius: 10rpx;
        }
      }
    }
    .work_order {
      width: 588rpx;
      height: 70rpx;
      background: rgba(255, 255, 255, 1);
      box-shadow: 0px 3rpx 6rpx rgba(0, 0, 0, 0.16);
      border-radius: 10rpx;
      margin: 0 auto;
      margin-top: 30rpx;
      display: flex;
      justify-content: space-around;
      align-items: center;
      position: relative;
      .order {
        font-size: 30rpx;
        font-family: PingFang SC;
        font-weight: 400;
        line-height: 42rpx;
        color: rgba(53, 53, 53, 1);
        opacity: 1;
        img {
          width: 38rpx;
          height: 29rpx;
          line-height: 42rpx;
        }
      }
      .border_order {
        width: 1rpx;
        background-color: #ccc;
        height: 45rpx;
        position: absolute;
      }
    }
    .portrait {
      width: 80rpx;
      height: 80rpx;
      border-radius: 50%;
      background-color: #fff;
      z-index: 1;
      position: fixed;
      bottom: 20rpx;
      right: 20rpx;
      .avatar {
        width: 100%;
        height: 100%;
        border-radius: 50%;
        z-index: 1;
      }
    }
  }
  .card_container {
    width: 750rpx;
    height: 100%;
    margin-top: 55rpx;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
    .card {
      margin-bottom: 20rpx;
      width: 680rpx;
      height: 214rpx;
      background: #fffffc;
      box-shadow: 0px 2rpx 8rpx rgba(53, 53, 53, 0.5);
      opacity: 1;
      padding: 20rpx;
      border-radius: 10rpx;

      .order_title {
        margin-top: 39rpx;
        display: flex;
        justify-content: space-between;
        align-items: center;
        font-size: 36rpx;
        font-family: PingFang SC;
        font-weight: bold;
        line-height: 50rpx;
        color: rgba(68, 68, 68, 1);
        opacity: 1;
      }
      .order_container {
        margin-top: 20rpx;
        display: flex;
        justify-content: space-between;
        align-items: center;
        flex-wrap: wrap;
        .warning_container {
          flex: 42%;
          margin-bottom: 10rpx;
          font-size: 28rpx;
        }
        .warning_container_r {
          flex: 58%;
          margin-bottom: 10rpx;
          font-size: 28rpx;
        }
      }
    }
  }
}
</style>
