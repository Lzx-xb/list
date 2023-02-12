<script setup>
defineProps({
  bg: {
    type: String,
    required: true,
  },
  count: {
    type: Number,
    required: true,
  },
});
</script>

<template>
  <div class="box" :style="{ backgroundColor: bg }">
    <span class="num">{{ count }}</span>
  </div>
</template>

<style scoped>
.box {
  width: 100%;
  height: 500px;
  display: flex;
  justify-content: center;
}
.num {
  font-weight: bolder;
}
</style>
