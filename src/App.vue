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
      playerList: new Map()
    }
  },
  components: {
    VoiceButton,
    FormalCard,
    Control
  },
  methods: {
    // 默认是可重叠播放的
      play(voice) {
        // todo ！！！播放逻辑
          // path 得到video路径
          // 放到里面可以在每次播放的时候灵活cdn和本地？ 为什么用const？
          // const voice = VoiceList.voices[0].voiceList[0]

          // 1.创建audio并随事件放入播放列表 以后好暂停
          const key = new Date().getTime()
          const path = `${CDN}/${voice.path}`
          let audio = new Audio(path)
          // key直接对应一个audio 不用.audio
          this.playerList.set(key,audio)
          // 2.播放
          audio.play()
          // 3.展示区
          this.nowPlay = voice
          // 4.播放结束后的善后
          audio.onended = () => {
            this.nowPlay = 'null'
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
          this.playerList.get(key).pause()
        }

        // 2.清空展示区
        this.nowPlay = 'null'


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
