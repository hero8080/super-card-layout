<script setup lang="ts">
import SuperCard from './components/SuperCard.vue'
</script>
<template>
  <SuperCard>
    <template v-slot:default="{item,index}">
      <div class="border">
        <span>{{ item }}</span>
      </div>
    </template>
  </SuperCard>
</template>

<style >
body,p{
  margin: 0;
  padding: 0;
}
.border{
  border: 1px solid red;
}
</style>
