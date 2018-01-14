# vue-image-upload-preview
[![npm version](https://img.shields.io/npm/v/vue-image-upload-preview.svg?style=flat)](https://www.npmjs.com/package/vue-image-upload-preview)

基于vue的图片上传预览插件

## Install

```shell
npm install vue-image-upload-preview --save-dev
```
## Usage

- 引入上传和预览组件，可按需引入
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
    </div>
    <!-- 图片预览 -->
    <image-preview
      style="z-index:200"
      :images="preImages"
      v-model="index"
      :numIsShow="true"
      :deleteIsShow="true"
      @delete="bindtap_delete"
    />
  </template>
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
       if(Array.isArray(res)){
          ...
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
    bindtap_delete(i){
      /*
       * this.images 存储预览图片路径
       * this.index 双向绑定当前显示的图片的下标
       * 下面是删除的例子
       */
      this.images = this.images.filter((v,i) => {
        return this.index!=i;
      })
    },
```
