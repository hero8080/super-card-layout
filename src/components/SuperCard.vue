<template>
  <div style="padding: 0 200px">
    <div :style="{margin: '0 -'+space+'px'}">
      <div class="super-card" ref="superCard">
        <div class="super-card-item" :style="{width:showWidth+'px',padding:space+'px'}" v-for="(item,index) in list">
          <slot :item="{item,index}">{{item}}</slot>
        </div>
      </div>
    </div>
  </div>
</template>
<script lang="ts">

import { nextTick, onBeforeUnmount, onMounted, onUnmounted, ref, watch} from "vue";

export default {
  name: "SuperCard",
  setup() {
    const list = ref([]);
    const debounce = (fn, delay) => {
      let timer = null //借助闭包
      return function() {
        if(timer){
          clearTimeout(timer) //进入该分支语句，说明当前正在一个计时过程中，并且又触发了相同事件。所以要取消当前的计时，重新开始计时
          timer = setTimeout(fn,delay)
        }else{
          timer = setTimeout(fn,delay) // 进入该分支说明当前并没有在计时，那么就开始一个计时
        }
      }
    }
    const setShowWidth = () => {
      nextTick(() => {
        if (superCard.value) {
          const offsetWidth = superCard.value.offsetWidth
          showWidth.value = offsetWidth / Math.floor(offsetWidth / minWidth.value);
        } else {
          showWidth.value = minWidth.value;
        }
      })
    }
    const fun=debounce(setShowWidth,300)
    const resizeObserver = new ResizeObserver(fun);


    onMounted(() => {

      resizeObserver.observe(superCard.value)

      setShowWidth();

      setInterval(() => {
        list.value.push(new Date());
      }, 1000)
    })
    onBeforeUnmount(()=>{
      resizeObserver.unobserve(superCard.value)
    })

    const minWidth = ref(300);
    const space = ref(0);
    const superCard = ref();
    const showWidth = ref(minWidth.value+space.value);

    watch(list.value, setShowWidth)
    return {
      list,
      minWidth,
      space,
      superCard,
      showWidth
    }
  }
}
</script>
<style scoped>
.super-card {
  display: flex;
  flex-wrap: wrap;
}

.super-card-item {
  box-sizing: border-box;
}
</style>
