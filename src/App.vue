<script setup lang='ts'>
import { useRouter } from 'vue-router'
const token = localStorage.getItem('_token')
const router = useRouter()
//进入页面先判断是否登陆着,localStorage.getItem('token')是登陆时候存的token
if (token === 'todayIsAGoodDay') {
  router.push('/home')
} else {
  router.push('/')
}
</script>

<template>
  <div class="app">
    <RouterView></RouterView>
  </div>
</template>

<style lang="less">
:root {
  --themeColor: #6f00ff;
}

body {
  padding: 0;
  margin: 0;
}

// .app {
//   width: 1024px;
//   margin: 0 auto;
//   text-align: center;
// }
</style>
