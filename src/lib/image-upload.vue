<template >
  <div class="upload-button" align="center">
    <input
    :style="{'font-size':touchSize+'rem'}"
    type="file"
    value=""
    accept="image/png,image/jpg,image/gif,image/jpeg"
    :multiple=multiple
    @change='bindchange_chooseImages'
    />
  </div>
</template>

<script>

import lrz from 'lrz';

export default {

  name:'image-upload',

  props:{
    // 上传图片字段名
    fieldName:'',
    // 上传地址
    url:'',
    // 是否支持多选
    multiple:{
      type:Boolean,
      default:false
    },
    // 接口是否支持多传
    multiUpload:{
      type:Boolean,
      default:false,
    },
    // 点击按钮的点击范围
    touchSize:{
      type:Number,
      default:0.5
    },
    // 压缩选项
    lrzOptions:{
      type:Object,
      default:function(){
        return {quality:0.7};
      }
    },
    // 上传其他参数
    data:{
      type:Object,
      default:function(){
        return {};
      }
    },
    // 限制张数
    maxCount:{
      type:Number,
      default:-1,//-1表示无限制
    }

  },

  methods:{
    /**
     *  绑定函数 -- 选择图片
     */
    bindchange_chooseImages(e){

      if (this.maxCount>=0 && e.target.files.length>this.maxCount) {
        return this.$emit('chooseImages','超过最大选择数量！');
      }

      this.parseImages(e.target.files)
      .then(res => {
        this.$emit('chooseImages',res);
      }).catch(e => {
        console.log(e);
      })
    },

    /**
     *  上传图片
     */
    uploadImages(images){
      if (this.multiUpload) {

      }else {
        let promises = [];
        images.forEach((v , i) => {
          if (!v.upload) {
            promises.push(this.uploadSingleImage(v.file));
          }
        })
        return Promise.all(promises)
      }

    },

    /**
     *  上传多张图片
     *  TODO 等服务器支持多传之后再改
     */
    uploadMultipleImages(){

    },

    /**
     *  上传单张图片
     *
     *  image：图片的file对象
     */
    uploadSingleImage(image){
      return new Promise((resolve , reject) => {
        var formData = new FormData();
        formData.append(this.fieldName, image);
        formData.append('type', 'image');
        $.ajax({
                  url: this.url,
                  type: 'POST',
                  dataType: 'JSON',
                  cache: false,
                  data: formData,
                  processData: false,
                  contentType: false,
                  success: (res) => {
                    resolve(res);
                  },
                  error: function(err) {
                    reject(err);
                  }
              });
      })
    },


    /**
     *  解析图片
     */
    parseImages(files){
      if (this.lrzOptions) {
        return this.compressImages(files);
      }else {
        return this.readImages(files);
      }
    },

    /**
     *  压缩图片
     */
    compressImages(files){
      let promises = [];
      for (var i = 0; i < files.length; i++) {
          promises.push(lrz(files[i],{width: 1024}));
      }
      return new Promise((resolve , reject) => {
        let images = [];
        Promise.all(promises)
        .then(res => {
          res.forEach(v => {
            images.push({file:v.file, src:v.base64, compress:true});
          })
          resolve(images);
        }).catch(e => {
          reject(e);
        })
      })
    },

    /**
     *  读取图片
     */
    readImages(files){
      return new Promise((resolve , reject) => {
        let images = [];
        for (var i = 0; i < files.length; i++) {
          let reader = new FileReader();
          !function (j) {
            reader.onload = function (e) {
              images[j] = {file:files[j] , src:e.target.result , compress:false};
              if (j === files.length-1) {
                resolve(images);
              }
            };
          }(i);
          reader.readAsDataURL(files[i]);
        }
      })
    },


  },
}
</script>

<style lang="css">
.upload-button{position: relative;display: inline-block;overflow: hidden;}
.upload-button input{position:absolute;left: 0rem; top: 0rem;opacity: 0.0;-ms-filter: 'alpha(opacity=0)';font-size: 0.5rem;}
</style>
