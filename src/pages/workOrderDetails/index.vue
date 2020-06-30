<template>
  <div>
    <div class="container">
      <van-nav-bar
        title="标题"
        left-arrow
        fixed
        @click-left="goBack"
      />
      <div class="header_box">
        <div class="title_text">
          {{ text_data.data.title }}
        </div>
        <div class="time">
          {{ text_data.data.warning_time }}
        </div>
      </div>
      <div class="particulars">
        <van-cell-group>
          <van-cell
            title="警报描述:"
            :value="text_data.data.describe"
            v-if="text_data.data.describe"
          />
          <van-cell
            title="警报地区:"
            :value="text_data.data.proxi"
            v-if="text_data.data.proxi"
          />
          <van-cell
            title="警报设备:"
            :value="text_data.data.equipment"
            v-if="text_data.data.equipment"
          />
          <van-cell
            title="设备IP:"
            :value="text_data.data.equipmen_id"
            v-if="text_data.data.equipmen_id"
          />
          <van-cell
            title="设备编号:"
            :value="text_data.data.equipmen_serial"
            v-if="text_data.data.equipmen_serial"
          />
        </van-cell-group>
      </div>
      <div
        v-for="(item, index) in text_data.list"
        :key="index"
        class="steps"
        v-if="text_data.list"
      >
        <img
          class="ico"
          :src="
            item.event == '前往维护'
              ? yello
              : item.event == '留言'
              ? blue
              : green
          "
          alt=""
        />
        <span class="time">{{ item.time }}</span>
        <div class="person" v-if="item.person">{{ item.person }}</div>
        <div class="event" v-if="item.event">{{ item.event }}</div>
        <div class="text" v-if="item.text != ''">{{ item.text }}</div>
        <div class="img_box" v-if="item.img">
          <img
            v-for="(ele, index) in item.img"
            :key="index"
            :src="ele"
            alt=""
          />
        </div>
      </div>

      <van-button type="primary" v-if="text_data.state != '已完成'"
        >进度沟通</van-button
      >
      <div v-if="text_data.data.is_look">
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
      code: "",
      active: 0,
      yello: "../../static/yello.png",
      blue: "../../static/blue.png",
      green: "../../static/green.png",
      //虚拟数据，留着没用
      text_data: {
        data: {},
        list: [
          {
            time: "2020/02/20 20:02",
            event: "前往维护",
            person: "[[张三]]",
            text: "13212312131321231313312",
            img: [
              "https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=2454953983,2544381202&fm=26&gp=0.jpg",
              "https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=2454953983,2544381202&fm=26&gp=0.jpg",
              "https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=2454953983,2544381202&fm=26&gp=0.jpg",
            ],
          },
          {
            time: "2020/02/20 20:02",
            event: "留言",
            person: "[张三]",
            text: "",
            img: [
              "https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=2454953983,2544381202&fm=26&gp=0.jpg",
              "https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=2454953983,2544381202&fm=26&gp=0.jpg",
              "https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=2454953983,2544381202&fm=26&gp=0.jpg",
            ],
          },
          {
            time: "2020/02/20 20:02",
            event: "维护完成",
            text: "",
            person: "[张三]",
            img: [
              "https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=2454953983,2544381202&fm=26&gp=0.jpg",
              "https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=2454953983,2544381202&fm=26&gp=0.jpg",
              "https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=2454953983,2544381202&fm=26&gp=0.jpg",
            ],
          },
        ],
        // 工单状态
        state: "未完成",
      },
    };
  },
  onLoad(e) {
    console.log(e);
    var _this = this;
    this.code = e.code;
    uni.request({
      url: `http://localhost:8080/wxapp/order/info_step`,
      data: {
        id: e.id,
      },
      success(res) {
        _this.text_data = res.data;
        console.log(res.data);
      },
		});
		
  },
  created() {},
  mounted() {},
  activited() {},
  update() {},
  beforeRouteUpdate() {},
  methods: {
    goBack() {
      // 判断在哪里进入的，直接返回
      var _this = this;
      var a = getCurrentPages();
      if (a.length == 1) {
        uni.switchTab({
          url: "/pages/home/index",
          success(res) {
            console.log(res);
          },
          fail(rej) {
            console.log(rej);
          },
        });
      }
      uni.navigateBack({
        delta: 1,
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
    margin-top: 188rpx;
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

.steps {
  margin: 30rpx;
  width: 750rpx;
  height: 100%;
  .ico {
    transform: translate(0, 5rpx);
    width: 36rpx;
    height: 36rpx;
    float: left;
  }
  .time {
    font-size: 32rpx;
    font-family: PingFang SC;
    font-weight: 400;
    line-height: 45rpx;
    color: rgba(51, 51, 51, 1);
    margin: 10rpx;
    opacity: 1;
  }
  .event {
    font-size: 32rpx;
    font-family: PingFang SC;
    font-weight: 400;
    line-height: 45rpx;
    color: rgba(128, 128, 128, 1);
    opacity: 1;
    margin: 10rpx;
    margin-left: 50rpx;
  }
  .person {
    font-size: 32rpx;
    font-family: PingFang SC;
    font-weight: 400;
    line-height: 45rpx;
    color: rgba(255, 162, 0, 1);
    opacity: 1;
    margin-left: 50rpx;
  }
  .text {
    word-wrap: break-word;
    word-break: break-all;
    margin-left: 50rpx;
    padding: 10rpx;
    width: 580rpx;
    background: rgba(225, 225, 230, 1);
    opacity: 1;
    border-radius: 10px;
  }
  .img_box {
    display: flex;
    justify-content: flex-start;
    align-items: center;
    padding: 20rpx;
    img {
      margin-left: 30rpx;
      width: 88rpx !important;
      height: 88rpx !important;
    }
  }
}
.van-button--normal {
  width: 690rpx;
  font-size: 36rpx !important;
  margin: 30rpx;
}
</style>
