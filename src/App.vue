<template>
  <div id="app">
    <Player/>

  </div>
</template>

<script>
import Player from './components/Player.vue'

export default {
  name: 'App',
  components: { Player },
  
}
</script>

<style lang="scss">
    // 主体背景   全局应用
    body {
      background: rgb(78, 78, 78);
      margin: 0;
      font-family: Helvetica Neue, Helvetica, Arial, sans-serif;
    }

    a {
      color: #337ab7;
      text-decoration: none;
    }
</style>
