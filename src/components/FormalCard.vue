<template>
  <div class="card-wrapper" :class="dark ? 'dark' : 'default'">
    <div class="card-header" v-if="$slots.header">
      <!-- 定义插槽 -->
      <slot name="header"></slot>
    </div>
    <div>
      <slot></slot>
    </div>
  </div>
</template>

<script>
    export default {
        props:['dark']
    }
</script>

<style lang="scss" scoped>
.card-wrapper {
  max-width: max-content;
  margin: 15px 30px;
  padding: 12px 17px;
  border-radius: 30px;
  overflow: hidden;
  transition: box-shadow 0.25s;
  box-shadow: 7px 7px 14px #3a3a3a, -7px -7px 14px #626262;
  color: #fff;

  .card-header {
    font-size: 20px;
    font-weight: 600;
    margin: 2px 0;
  }
}

.default {
  background: rgb(255, 125, 125);
  &:hover {
    box-shadow: 0 10px 10px 0px rgba(0, 0, 0, 0.3);
  }
}

.dark {
  background: rgb(78, 78, 78);
  &:hover {
    background-image: linear-gradient(to right bottom, #464646, #535353);
  }
}
</style>
