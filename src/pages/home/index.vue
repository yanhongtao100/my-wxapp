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
      <div class="work_order" v-if="identity != 2">
        <div class="order" @click="filter(1)">
          <img src="/static/组 38@2x.png" alt="" />
          <span class="order_filter" :class="{ bottom_border: is_show_left }"
            >警报工单</span
          >
        </div>
        <div class="order" @click="filter(0)">
          <img src="/static/组 39@2x.png" alt="" />
          <span class="order_filter" :class="{ bottom_border: is_show_right }"
            >人工工单</span
          >
        </div>
        <div class="border_order"></div>
      </div>
    </div>

    <div class="card_container">
      <div
        class="card"
        v-for="(item, index) in info.data"
        :key="index"
        @click="onLook(item)"
      >
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
      code: "", //
      identity: 2, //人员权限
      info: {}, //工单资料
      copyinfo: {},
      user: {}, //用户资料
      is_underway: true,
      is_peading: false,
      is_finish: false,
      is_show_left: false,
      is_show_right: false,
    };
  },
  onLoad() {
    let _this = this;
    //因为tabbar里无法传参数，所以只好在这里重新获取code值，
    //然后再请求用户权限信息和工单信息，可怕的回调地狱啊
    uni.login({
      success(res) {
        _this.code = res.code;
        uni.request({
          url: `http://localhost:8080/wxapp/user/info`,
          data: {
            code: _this.code,
          },
          success(res) {
            _this.identity = res.data.identity;
            // 如果是管理员????
            // if (res.data.identity === 0) {
              uni.request({
                url: `http://localhost:8080/wxapp/home/msg`,
                data: {
                  code: _this.code,
                  event: 1,
                },
                success(re) {
                  _this.info = re.data;
                  _this.copyinfo.data = [...re.data.data];
                },
              });
            // } else if (res.data.identity === 1) {
            // } else if (res.data.identity === 2) {
            // } else {
            //   uni.showToast({
            //     title: "参数错误",
            //     icon: "none",
            //   });
            // }
          },
        });
      },
      fali(rej) {
        uni.showToast({
          title: "请求失败，请重试",
          icon: "none",
        });
      },
    });
  },
  created() {
    let _this = this;
    uni.getUserInfo({
      success(res) {
        _this.user = JSON.parse(res.rawData);
      },
    });
  },
  mounted() {},
  activited() {},
  update() {},
  beforeRouteUpdate() {},
  methods: {
    //筛选方法

    filter(a) {
      try {
        this.info.data = [...this.copyinfo.data];
        this.copyinfo.data = [...this.info.data];
        this.info.data = [];
        this.copyinfo.data.forEach((element, i) => {
          if (element.is_enviroment == a) {
            this.info.data.push(element);
          }
        });
        if (a == 1) {
          this.is_show_left = true;
          this.is_show_right = false;
        }
        if (a == 0) {
          this.is_show_left = false;
          this.is_show_right = true;
        }
      } catch (error) {
				throw error
			}
    },
    onLook(data) {
      uni.redirectTo({
        url: `/pages/workOrderDetails/index?id=${data.id}`,
        fail(rej) {
          uni.showToast({
            title: "网络错误，请重试",
            icon: "none",
          });
        },
      });
    },
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
          that.copyinfo.data = [...re.data.data];
          that.is_show_left = false;
          that.is_show_right = false;
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
          that.copyinfo.data = [...re.data.data];
          that.is_show_left = false;
          that.is_show_right = false;
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
          that.copyinfo.data = [...re.data.data];
          that.is_show_left = false;
          that.is_show_right = false;
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
.order_filter {
  padding: 12rpx 20rpx;
}
.bottom_border {
  border-bottom: 6rpx solid rgba(180,210,249,1);
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
