<template>
  <div id="app">
    <img src="./assets/logo.png">
    <h1>{{ msg }}</h1>
    <h2>Essential Links</h2>
    <image-upload
      class="image_upload"
      url='https://tsesb.yunjuhe.com.cn/medicalServer/doctor/uploadServerImage'
      :touch-size = 1
      :multiple = true
      field-name = 'serverImgFile'
      :max-count = 10
      @chooseImages='bindtap_chooseImages'
    />

    <img :src="image.src" alt="" v-for="(image , i) in images" @click="bingtap_preview(i)">

    <!-- 图片预览 -->
    <image-preview
      style="z-index:200"
      :images="preImages"
      v-model="index"
      :numIsShow="true"
      :deleteIsShow="true"
      @delete="bindtap_delete"
    />

    <div @click="bindtap_upload">
      上传
    </div>

  </div>
</template>

<script>

import {ImageUpload , ImagePreview} from './lib/index.js'

export default {
  name: 'app',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      images:[],
      index:-1,
    }
  },

  computed:{
    // 预览图片路径
    preImages(){
      return this.images.map(v=>{return v.src});
    },
  },

  methods:{
    /**
     *  绑定函数 -- 选择图片
     */
    bindtap_chooseImages(res){
      if (Array.isArray(res)) {
        this.images = this.images.concat(res);
      }else {
        console.log(res);
      }
    },

    /**
     *  绑定函数 -- 删除图片
     */
    bindtap_delete(){
      this.images = this.images.filter((v,i) => {
        return this.index!=i;
      })
    },

    /**
     *  绑定函数 -- 预览图片
     */
    bingtap_preview(i){
      this.index = i;
    },

    /**
     *  绑定函数 -- 取消预览
     */
    bingtap_hiddenImg() {
      this.index = -1;
    },

    /**
     *  绑定函数 -- 上传图片
     */
    bindtap_upload(){
      this.$refs.imgaeUpload.uploadImages(this.images)
      .then(res => {
        console.log(res);
      })catch(err => {
        console.log(err);
      })
    }
  },


  components: {
    'image-preview':ImagePreview,
    'image-upload':ImageUpload
  }
}
</script>

<style>
*{padding:0;margin:0}
img{height: 60px;width: 60px}

.image_upload{  height: 60px; width: 60px;background: red}

</style>
