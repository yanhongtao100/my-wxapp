<template>
  <div class="container">
    <van-nav-bar title="标题" left-text="返回" left-arrow />
    <h4>创建工单</h4>
    <div class="border"></div>
    <van-cell-group>
      <h2 class="lab">工单描述1</h2>
      <van-field
        class="text_input"
        :value="value"
        border=false 
        placeholder="请输入用户名"
        @change="onChange"
      />
    </van-cell-group>
    <van-cell-group>
      <h2 class="lab">工单描述1</h2>
      <van-field
        class="text_input"
        :value="value"
        border=false 
        placeholder="请输入用户名"
        @change="onChange"
      />
    </van-cell-group>
    <van-cell-group>
      <h2 class="lab">工单描述1</h2>
      <van-field
        class="text_input"
        :value="value"
        border=false 
        placeholder="请输入用户名"
        @change="onChange"
      />
    </van-cell-group>
    <van-cell-group>
     <h2 class="lab">工单描述1</h2>
      <van-field
        class="text_input"
        :value="value"
        border=false 
        placeholder="请输入用户名"
        @change="onChange"
      />
    </van-cell-group>
    <div class="space"></div>
    <van-cell-group>
      <h2 class="lab">工单描述1</h2>
      <van-field
        class="text_input"
        :value="value"
        border=false 
        placeholder="请输入用户名"
        @change="onChange"
      />
    </van-cell-group>
    <van-button type="primary">指定负责人</van-button>
  </div>
</template>

<script>
export default {
  name: "",
  components: {},
  props: {},
  data() {
    return {
      value: "",
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
.van-cell {
  padding-left: 0 !important;
}
.container {
  width: 750rpx;
  height: 100%;
  min-height: 100vh;
  background: rgba(239, 239, 244, 1);
  h4 {
    padding: 30rpx;
    height: 47rpx;
    line-height: 87rpx;
    width: 750rpx;
    font-size: 26rpx;
    font-family: PingFang SC;
    font-weight: 400;
    background-color: #fff;
    color: rgba(153, 153, 153, 1);
    opacity: 1;
  }
  .lab {
    padding: 30rpx;
    padding-bottom: 0;
    background-color: #fff;
    margin-top: 0rpx;
    font-size: 34rpx;
    font-family: PingFang SC;
    font-weight: 400;
    line-height: 48rpx;
    color: rgba(34, 34, 34, 1);
    opacity: 1;
  }
  .text_input {
    height: 61rpx;
  }
  .van-field__input {
    padding-left: 35rpx !important;
  }
  .space {
    width: 750rpx;
    height: 10rpx;
  }
}
.van-button--normal {
  width: 690rpx;
  font-size: 36rpx !important;
  margin: 30rpx;
}
</style>
