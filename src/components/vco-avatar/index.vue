<template>
  <div class="vco-avatar" :class="{'round': round}" :style="{width: size + 'px', height: size + 'px'}">
    <div class="content" :class="{'loaded': loaded}">
      <img v-if="isSuc" :src="src" :class="{'height-fixed': heightFixed}">
      <img v-else src="./../../assets/images/user.svg" alt="">
    </div>
  </div>
</template>

<script setup>
  import { onMounted, ref } from 'vue'
  
  const props = defineProps({
    src: {
      type: String,
      default: ''
    },
    round: {
      type: Boolean,
      default: true
    },
    size: {
      type: Number,
      default: 50
    }
  })

  const loaded = ref(false)
  const isSuc = ref(false)
  const heightFixed = ref(false)

  const setImageSize = () => {
    if (props.src) {
      const img = new Image();
      img.src = props.src

      // 等待图片加载完成后获取宽高
      img.onload = () => {
        heightFixed.value = img.width > img.height
        isSuc.value = true
        loaded.value = true
      };

      img.error = () => {
        loaded.value = true
      };

    } else {
      loaded.value = true
    }
  }

  onMounted(() => {
    setImageSize()
  })
</script>

<style lang="less" scoped>
  .vco-avatar {
    overflow: hidden;
    background-color: #ccc;
    &.round {
      border-radius: 50%;
    }
    > .content {
      width: 100%;
      height: 100%;
      overflow: hidden;
      display: flex;
      align-items: center;
      justify-content: center;
      opacity: 0;
      transition: 0.3s;
      &.loaded {
        opacity: 1;
      }
      > img {
        max-width: 1000px;
        width: 100%;
        height: auto;
        &.height-fixed {
          height: 100% !important;
          width: auto !important;
        }
      }
    }
  }
</style>