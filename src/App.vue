<template>
  <div id="app">
    <!-- 当前播放展示区 -->
    <div v-if="nowPlay.name" class="status">
        {{ nowPlay.name }}
    </div>

    <!-- 按钮区 -->
    <div v-for="category in voices" :key="category.categoryName">
        <!-- 使用插槽不能使用自闭合标签 必须两边写全 -->
        <FormalCard :dark="true">
            <template #header>
              {{ category.categoryName }}
            </template>
          
          <!-- 全部的按钮 遍历 默认插槽位置-->
            <VoiceButton
                v-for="voice in category.voiceList" :key="voice.name"
                :value="voice.name"
                @click.native="play(voice)"
            />
        </FormalCard>
    </div>

    <Control>
      <div @click="stopPlay">
        <img src="@/assets/images/stop.svg" />
      </div>
                                      <!-- 可重叠则被选中状态的样式 -->
      <div @click="changeOverlap" :class="{ selected: overlap }">
        <img src="@/assets/images/over.svg" />
      </div>
    </Control>


  </div>
</template>

<script>
import VoiceButton from './components/VoiceButton.vue'
import VoiceList from '@/../setting/voices.json'
import FormalCard from './components/FormalCard.vue'
import Control from './components/Control.vue'


const CDN = "https://cdn.jsdelivr.net/gh/vbup-osc/mio-button@master/public/voices"

export default {
  name: 'App',
  data(){
    return{
      // category:VoiceList.voices,
      // 不能在category基础上在来一个数据嘛
      voices:VoiceList.voices,
      // nowPlay是一个对象，存放正在播放的voiceItem
      nowPlay:{},
      // 放弃了，加一个播放列表存储每个正在播放的音频 播放完的清除
      playerList: new Map(),
      overlap:false
    }
  },
  components: {
    VoiceButton,
    FormalCard,
    Control
  },
  methods: {
    // 默认是可重叠播放的

    // 不重叠播放：在播放下一个前把前一个暂停掉
    // 要暂停掉上一个就需要找到上一个，在没有playerlist或者audio标签的情况下是找不到上一个的
    // 创建playerlist后 从playerlist找到上一个音频 然后暂停
      play(voice) {
          // 设定不同情况下的key 不能重叠时，都设定为once，而从保证播放列表里只有一条音频
          // 在外部设置key变量
          let key = ''
          if(!this.overlap){
            // 第一次播放，playerlist里还没有值
            // 当有once这个key后暂停它
            // 但不是第一次的情况下里面有once值就停止播放
            // 但是之前还可能有key是时间戳的情况
            // 此时之前在播放列表里的音声还会播放
            // 需要取消掉
            this.stopPlay()
            key = 'once'
          }else{
            // 重复播放的情况不用多做处理，默认就是重复播放状态
            // 这里传入key是为了特定每个audio 好在以后同时暂停
            key = new Date().getTime()
          }
        // todo ！！！播放逻辑
          // path 得到video路径
          // 放到里面可以在每次播放的时候灵活cdn和本地？ 为什么用const？
          // const voice = VoiceList.voices[0].voiceList[0]

          // 1.创建audio并随事件放入播放列表 以后好暂停
          // const key = new Date().getTime()
          const path = `${CDN}/${voice.path}`
          let audio = new Audio(path)
          // key直接对应一个audio 不用.audio
          this.playerList.set(key,audio)
          // 2.播放
          audio.play()
          // 3.展示区
          this.nowPlay = voice
          // 4.播放结束后的善后
          // 播放时就约定好 播放结束后清空展示区 离开播放列表
          audio.onended = () => {
            this.nowPlay = {}
            this.playerList.delete(key)
          }


          // 返回一个匿名函数
          // 闭包
          // 将audio传给另一个函数

          // todo 重要！！ 一定要传给这个返回的函数参数
      },
      stopPlay(){
        // todo 闭包的应用！！ 看来用不了闭包
        // this.play(undefined)()

        // 1.暂停所有key对应的audio
        for (const key of this.playerList.keys()) {
          // todo 因为只是暂停 没有播放结束 所以没有触发结束事件出列表嘛？
          this.playerList.get(key).pause()
        }

        // 2.清空展示区和播放列表
        this.nowPlay = 'null'
        this.playerList.clear()
      },
      changeOverlap(){
        this.overlap = !this.overlap
      }


  }
}
</script>

<style lang="scss" scoped>
.status {
  position: fixed;
  top: 10px;
  left: 30px;
  padding: 5px 20px;
  background: #585858;
  border-radius: 150px;
  color: #fff;
  box-shadow: 0 10px 10px 0px rgba(0, 0, 0, 0.2);
  text-align: center;
  font-size: 14px;
  font-weight: 600;
}

.selected {
  background: #4e4e4e !important;
}
</style>
