<script setup>
import { onBeforeUnmount, onMounted, reactive } from "vue";
import HelloWorld from "./components/HelloWorld.vue";
let list = ["rgb(233,32,38)"];
let loading = false;
let loadingMsg = "下拉加载更多数据";
let menu = reactive({ list, loading, loadingMsg });

onMounted(async () => {
  //监听scroll
  window.addEventListener("scroll", debounce(scrollBottom, 1000));
  getList(9); //初始化数据
});

onBeforeUnmount(() => {
  window.removeEventListener("scroll", () => {
    console.log("removeEvenListener");
  });
});

async function getList(num) {
  menu.loading = true;
  //模拟接口响应时间
  let time = Math.floor(Math.random() * 5000);
  console.log("time = " + time);
  await setTimeout(() => {
    for (let i = 0; i < num; i++) {
      let r = Math.floor(Math.random() * 256);
      let g = Math.floor(Math.random() * 256);
      let b = Math.floor(Math.random() * 256);
      let rgb = "rgb(" + r + "," + g + "," + b + ")";
      menu.list.push(rgb);
    }
    menu.loading = false;
  }, time);
}
// 防抖，下拉到底部防止多次触发调用getList(); time = 1000;
function debounce(func, time) {
  let timeout;
  return function () {
    let context = this;
    let args = arguments;
    if (timeout) clearTimeout(timeout);
    timeout = setTimeout(() => {
      func.apply(context, args);
    }, time);
  };
}

function scrollBottom() {
  menu.loading = true;
  let scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
  let clientHeight = document.documentElement.clientHeight;
  let scrollHeight = document.documentElement.scrollHeight;
  let bottomOfWindow = parseInt(scrollTop + clientHeight) >= scrollHeight;
  if (scrollTop != 0 && bottomOfWindow && menu.loading == true) {
    if (menu.list.length < 50) {
      getList(10);
    } else if (menu.list.length >= 50) {
      menu.loading = false;
      menu.loadingMsg = "没有更多数据了";
    }
  }
}
</script>

<template>
  <header>
    <div class="wrapper" v-for="(item, index) in menu.list" :key="index">
      <HelloWorld :bg="item" :count="index" />
      <div style="margin-bottom: 10px"></div>
    </div>
    <div class="loading-box">
      <div class="loading" v-show="menu.loading"></div>
      <div v-show="!menu.loading">{{ menu.loadingMsg }}</div>
    </div>
  </header>
</template>

<style scoped>
@import "./assets/loading.css";
.loading-box {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 50px;
}
header {
  line-height: 1.5;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
