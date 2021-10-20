<template>
<!-- 为什么要“‘btn’”？ -->
  <div class="btn-wrapper" :class="'btn'">
    <!-- 有链接就是a标签 没有就显示字符串 -->
    <template>
      {{ value }}
    </template>
  </div>
</template>

<script>
    export default {
      data(){
        return{
          // val:'Play'

        }
      },
      // props列表里的值需要加引号
      props:['value']
    }
</script>

<style lang="scss" scoped>
a {
  color: #fff;
}

.btn-wrapper {
  display: inline-block;
  font-size: 14px;
  font-weight: 600;
  max-width: max-content;
  word-break: break-all;
  padding: 6px 12px;
  margin: 5px;
  border-radius: 17px;
  transition: all 0.3s;
  user-select: none;
  cursor: pointer;
}

.btn {
  background: #ff9c9c;
  box-shadow: 5px 5px 10px #d16767, -5px -5px 10px #ff9494;
  &:hover {
    box-shadow: 0 0 5px 5px rgba(255, 255, 255, 0.397);
  }
  &:active {
    box-shadow: inset 0 3px 5px rgba(0, 0, 0, 0.125);
  }
}

.voice-btn {
  background: #ff7c7c;
  box-shadow: 4px 4px 10px #343434, -4px -4px 10px #686868;
  &:hover {
    box-shadow: 0 0 5px 5px rgba(252, 120, 120, 0.322);
  }
  &:active {
    box-shadow: inset 3px 3px 5px #3c3c3c, inset -3px -3px 5px #606060;
  }
}
</style>

