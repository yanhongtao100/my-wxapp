<template>
  <div class="container">
    <van-nav-bar title="标题" left-arrow fixed @click-left="onClickLeft" />
    <div class="area">
      <van-cell-group>
        <van-cell size="large">
          <view slot="title">
            <view class="van-cell-text">单元格</view>
            <input type="text" :value="value" placeholder="在此输入工单名称" />
          </view>
        </van-cell>
      </van-cell-group>
      <van-cell-group>
        <van-cell size="large">
          <view slot="title">
            <view class="van-cell-text">工单描述</view>
            <van-field
              :value="message"
              type="textarea"
              border="{{ false }}"
              placeholder="在此输入工单名称"
              autosize
            />
          </view>
        </van-cell>
      </van-cell-group>
    </div>
    <div class="scan">
      <div class="cont" @click="scancode">
        <img src="/static/组107@2x.png" alt="" />
        <span>扫描二维码</span>
      </div>
    </div>
    <div class="select">
      <van-cell
        title="选定负责人"
        :value="personnel"
        size="large"
        is-link
        @click="selectPersonnel"
      />
      <van-popup
        :show="isShow"
        position="bottom"
        custom-style="height: 40%;"
        overlay
        close-on-click-overlay
        @close="onClose"
      >
        <van-picker
          :columns="columns"
          @change="onChange"
          show-toolbar
          title="标题"
          @cancel="onCancel"
          @confirm="onCancel"
        />
      </van-popup>
    </div>
    <div class="btn">
      <van-button type="primary" block class="btn">创建工单</van-button>
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
      columns: ["杭州", "宁波", "温州", "嘉兴", "湖州"],
      isShow: false, //遮罩层默认关闭
      personnel: "未指定",
      value: "",
      message: "",
    };
  },
  created() {},
  mounted() {},
  activited() {},
  update() {},
  beforeRouteUpdate() {},
  methods: {
    scancode() {
      uni.scanCode({
        success(res) {
          console.log(res);
        },
      });

      // 只允许从相机扫码
      uni.scanCode({
        onlyFromCamera: true,
        success(res) {
          console.log(res);
        },
      });
    },
    onCancel() {
      this.isShow = false;
    },
    onChange(e) {
      console.log(e);
      this.personnel = e.detail.value;
    },
    onClose() {
      this.isShow = false;
    },
    // 选择负责人事件
    selectPersonnel() {
      this.isShow = true;
    },
    // 返回事件
    onClickLeft() {
      uni.switchTab({
        url: "/pages/my/index",
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
  width: 750rpx;
  height: 100vh- 69px;

  background: rgb(235, 235, 240);
  .area {
    background: rgba(255, 255, 255, 1);
    margin-top: 138rpx;
    width: 750rpx;
    height: 100%;
    padding: 15rpx;
  }
  .scan {
    margin-top: 20rpx;
    background: rgba(255, 255, 255, 1);
    width: 750rpx;
    height: 95rpx;
    display: flex;
    justify-content: center;
    align-items: center;
    .cont {
      display: flex;
      justify-content: center;
      align-items: center;
    }
    img {
      margin-right: 15rpx;
      width: 40rpx;
      height: 40rpx;
    }
  }
  .select {
    margin-top: 20rpx;
    background: rgba(255, 255, 255, 1);
    width: 750rpx;
    padding: 15rpx;
  }
  .btn {
    width: 710rpx;
    padding: 0 20rpx;
  }
}
/deep/.van-cell {
  padding-left: 0 !important;
  width: 720rpx !important;
}
</style>
