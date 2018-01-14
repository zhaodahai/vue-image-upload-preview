
<template>
  <transition name='shade'>
    <div class="shade flex-column-center" :value="value" v-show="visible" @touchmove.prevent >
      <div v-if="deleteIsShow||numIsShow" class="top-view flex-center">
        <p v-if="numIsShow">{{'( '+((value>0?value:index)+1)+' / '+images.length+' )'}}</p>
        <img v-if="deleteIsShow" src="../assets/delete.png" alt="" @click="bindtap_delete()">
      </div>
      <div class="image" :class="{image_full:!deleteIsShow&&!numIsShow}">
        <mt-swipe
          ref="imgSwipe"
          v-if="visible"
          :show-indicators="false"
          :auto="0"
          :continuous="false"
          :defaultIndex="value"
          @change="bindChange_index">
          <mt-swipe-item v-for="(image ,i) in images" :key="image">
            <img :src="image" alt="" @click='bingtap_hidden'>
          </mt-swipe-item>
        </mt-swipe>
      </div>
    </div>
  </transition>
</template>

<script>

import {Swipe, SwipeItem} from 'mint-ui';

export default {
  name:'image-preview',

  data(){
    return {
      // 是否显示
      visible:false,
      // 暂存index
      index:-1,
    }
  },

  props:{
    // 所有图片路径
    images:{
      type:Array,
      default:[]
    },
    // 删除按钮是否显示
    deleteIsShow:{
      type:Boolean,
      default:false
    },
    // 数字是否显示
    numIsShow:{
      type:Boolean,
      default:false
    },
    // 双向绑定value
    value:{
      type:Number,
      default:-1,
    }
  },

  methods:{

    bindChange_index(index){
      this.$emit('input' , index);
      this.$emit('change' , index);
    },

    bingtap_hidden(){
      this.$emit('input' , -1);
    },

    bindtap_delete(){
      this.$emit('delete');
    },
  },

  watch:{
    value(v){
      v >= 0 && (this.index = v);
      this.visible = (v >= 0);
    },
    images(v){
      v.length<=0 && (this.visible = false);
    }
  },

  components:{
    'mt-swipe':Swipe,
    'mt-swipe-item':SwipeItem,
  }
}
</script>

<style lang="css">
.flex-center{display: flex;flex-direction: row;align-items: center;justify-content: center;}
.flex-row-center{ display: flex;flex-direction: row;align-items: center;}
.flex-column-center{display: flex;flex-direction: column; align-items: center;}
/* 遮罩 */
.shade{  width: 100%;height: 100%;position: fixed;top: 0;left: 0;background-color: rgba(0, 0, 0, 0.9);overflow: hidden;z-index: 100;transition:all 0.3s;}
.shade-enter-active, .shade-leave-active {transition: opacity .5s}
.shade-enter, .shade-leave-active {opacity: 0}
.shade .top-view{height: 120px;width: 100%;position: relative}
.shade .top-view p{color: white;font-size: 40px}
.shade .top-view img{width: 50px;height: 50px;position: absolute;right: 60px}
.shade .image {width: 100%;height: 80%;position: relative;}
.shade .image_full{height: 100%}
.shade .image img{position: absolute;top: 0;left: 0;height: 100%;width: 100%}
</style>
