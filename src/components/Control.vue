<template>
 <div class="control-wrapper">
   <!-- 展开收纳按钮 menu-->
    <div @click="showItem(isShowItem)" class="menu-btn">
      <img v-if="isShowItem" src="@/assets/images/close.svg" />
      <img v-else src="@/assets/images/menu.svg" />
    </div>
    <!-- 具体控制按钮 这里用slot插值 默认不展开 width为0 -->
    <!-- control组件 只提供vue 不提供方法 具体方法到使用它的组件里实现 -->
    <div class="btn-list" ref="btnList">
      <slot></slot>
    </div>
  </div> 
</template>

<script>
export default {
    data() {
      return {
        isShowItem:false,
        playList:new Map()
      }
    },
    methods: {
      // todo 当按钮被按下，先根据当前按钮状态决定是展开还是收纳
      // 然后再将按钮的状态反转


      // 针对不同事态 采取的对应的行动是一样的
      // 如果isShowItem是false，那么就展开，然后设置为true
      // 不管之前的控制栏实际是展开还是收纳
      showItem(isShowItem){
        // 采取状态对应的动作
        // 分支的条件确定了变量什么状态 采取什么条件
        // isShowItem是前态，根据前态的展示与否，决定点击后展开还是收纳
        // if (isShowItem) {
        //   // 当isShowItem的值是true的时候
        //   this.$refs.btnList.style.width = 0
          
        // } else {
          
        //   this.$refs.btnList.style.width = this.$refs.btnList.children.length * 46 + 'px'
        // }

        // 1.根据状态决定展开还是收纳
        // isShowItem可真可假，当是真的时候，采取对应的行为

        // 在false状态下展开，就是在isShowItem等于false状态下加长度
        // if(isShowItem === false) if(!isShowItem === true) if(!isShowItem)等价
        // 想要执行哪个分支下的代码，就需要让哪个条件为真

        if (!isShowItem) {
          // 当isShowItem的值是true的时候
          this.$refs.btnList.style.width = this.$refs.btnList.children.length * 46 + 'px'
        } else {
          this.$refs.btnList.style.width = 0
        }

        // 2.改变状态
        this.isShowItem = !this.isShowItem
      }
    },
    mounted() {
      // 加载完后默认打开控制栏
      this.showItem(false)
    },
}
</script>

<style lang="scss" scoped>
.control-wrapper {
  position: fixed;
  right: 15px;
  bottom: 15px;
  display: flex;
  flex-direction: row-reverse;
  align-items: center;
  user-select: none;
  .menu-btn {
    z-index: 2;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 55px;
    height: 40px;
    margin: 0 0 0 5px;
    background: rgba(255, 123, 123, 0.76);
    color: #fff;
    border-radius: 30px;
    cursor: pointer;
    &:hover {
      background-color: rgba(255, 123, 123, 0.9);
    }
    img {
      width: 30px;
    }
  }
  .btn-list {
    overflow: hidden;
    z-index: 1;
    display: flex;
    flex-direction: row-reverse;
    align-items: center;
    // 一开始宽度是零，实现渐进效果
    width: 0;
    transition: width 0.3s;
    div {
      display: flex;
      align-items: center;
      justify-content: center;
      width: 36px;
      height: 36px;
      background: rgba(255, 123, 123, 0.76);
      border-radius: 50%;
      margin: auto 5px;
      flex-shrink: 0;
      box-shadow: 1px 1px 5px #343434;
      cursor: pointer;
      &:hover {
        background-color: rgba(255, 123, 123, 0.9);
      }
    }
  }
}
</style>