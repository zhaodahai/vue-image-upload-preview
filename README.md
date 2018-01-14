# vue-image-upload-preview
[![npm version](https://img.shields.io/npm/v/vue-image-upload-preview.svg?style=flat)](https://www.npmjs.com/package/vue-image-upload-preview)

基于vue的图片上传和预览插件

该组件引用了 [mint-ui](https://github.com/ElemeFE/mint-ui) 和 [lrz](https://github.com/think2011/localResizeIMG)

## Install

```shell
npm install vue-image-upload-preview --save-dev
```
## Usage

- 引入图片上传和预览组件，可按需引入
```js
  import {ImageUpload , ImagePreview} from 'vue-image-upload-preview'
```

- 注册组件
```js
  components:{
    'image-preview':ImagePreview,
    'image-upload':ImageUpload
  }
```

- 使用组件
```html
  <template>
    <div>
      <!-- 图片上传 -->
      <image-upload
       class="image-upload"
       ref='imgaeUpload'
       :url='http://XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX'
       :touch-size = 1
       :multiple = true
       :lrz-options = {width:1024}
       field-name = 'serverImgFile'
       :data = {}
       :max-count = 10
       @chooseImages='bindtap_chooseImages'
      />

      <!-- 图片预览 -->
      <image-preview
        style="z-index:200"
        :images="preImages"
        v-model="index"
        :numIsShow="true"
        :deleteIsShow="true"
        @delete="bindtap_delete"
      />

    </div>
  </template>
```

- 绑定数据
```js
    data() {
      return {
        index: -1,
        images: []
      }
    },

    computed:{
      preImages() {
        return this.images.map(v=>{return v.src});
      }
    }
```

- 上传图片
```js
  this.$refs.imgaeUpload.uploadImages(this.images)
  .then(res => {
    ...
  }).catch(res => {
    ...
  })
```

- 自定义上传按钮样式
```css
  .image_upload{  height: 60px; width: 60px;background: red}
```

## Props

##### ImageUpload
- `url` - `String` - 上传图片的路径；
- `field-name` - `Stirng` - 上传图片的字段名；
- `multiple` - `Boolean` - [default:`false`] - 是否支持图片多选；
- `lrz-options` - `Object` - [default:`{quality:0.7}`] - 图片压缩选项，参考（https://github.com/think2011/localResizeIMG/wiki/2.-参数文档)；
- `max-count` - `Number` - [default:`-1`] - -1表示无限张；
- `chooseImages` - `Function` - 选择图片回调；
```js
    bindtap_chooseImages(e){
      /*
       * 选择成功 e是一个数组
       * e[0].file 图片文件对象，用于上传
       * e[0].src 图片base64，用于预览
       * e[0].compress 图片是否经过压缩
       */
       if(Array.isArray(e)){
          this.images = this.images.concat(e);
       }else {
          console.log(e);
       }
    }
```
##### ImagePreview
- `images` - `Array` - [default:`[]`] - 预览图片的所有路径；
- `deleteIsShow` - `Boolean` - [default:`false`] - 是否显示删除按钮；
- `numIsShow` - `Boolean` - [default:`false`] - 是否显示数字；
- `chooseImages` - `Function` - 选择图片回调；
```js
    bindtap_delete(e){
      /*
       * e 当前显示的图片的下标(双向绑定index，可以忽略e)
       * this.images 存储选择图片传过来的对象
       * this.index 双向绑定当前显示的图片的下标
       * 下面是删除的例子
       */
      this.images = this.images.filter((v,i) => {
        return this.index!=i;
      })
    },
```


## Notice
- 使用方法仅供参考
- /src/App.vue 有简单的例子 [demo](https://github.com/zhaodahai/vue-image-upload-preview/blob/master/src/App.vue)
