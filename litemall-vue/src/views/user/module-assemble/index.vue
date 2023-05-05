<template>
  <div>
<van-cell-group title="安装类型">
    <van-cell class="order-coupon" :title="type" is-link arrow-direction="down" @click="showList = true" />
</van-cell-group>
<van-cell-group title="安装信息">

  <van-field v-model="content" 
    clearable autosize center
    placeholder="请告诉我们需安装电器种类、品牌..." 
    type="textarea"
    rows="10"
    size="large"
    />
</van-cell-group>

<van-cell-group title="联系方式">

   <van-field size="large" v-model="mobile" placeholder="请输入联系电话，方便我们与您联系" />
   </van-cell-group>

<van-button size="large" type="primary" @click="submit">提交</van-button>


<van-popup v-model="showList" position="bottom">
<van-picker :columns="types" @change="onType" />
</van-popup>
  </div>
</template>

<script>
import { Field , Picker, Popup, Button } from 'vant';
import { feedbackAdd } from '@/api/api';

export default {
  data() {
    return {
      mobile: '',
      content: '',
      showList: false,
      types:['平台电器安装', '自购电器安装', '安装质量问题'],
      type: ''
    };
  },
  created() {
  },
  methods: {
    onType(picker, value, index) {
      this.type = value
      this.showList = false
    },
    submit() {
      if(this.mobile === ''){
        this.$toast("请输入联系电话");
        return;
      }
      if(this.type === ''){
        this.$toast("请选择安装类型");
        return;
      }
      if(this.content === ''){
        this.$toast("请输入安装信息");
        return;
      }      
      feedbackAdd({ mobile: this.mobile, feedType: this.type, content: this.content}).then(res => {
        this.$toast("感谢您的使用！");
        this.$router.go(-1);
      })
    }
  },

  components: {
    [Field.name]: Field,
    [Popup.name]: Popup,
    [Button.name]: Button,
    [Picker.name]: Picker
  }
};
</script>


<style lang="scss" scoped>
.addressGroup {
  margin-bottom: 10px;
  &:last-child {
    margin-bottom: 0;
  }
}

.bottom_btn {
  position: fixed;
  bottom: 0;
}
</style>
