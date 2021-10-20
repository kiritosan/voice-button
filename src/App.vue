<template>
  <div id="app">
    <div v-for="category in voices" :key="category.categoryName">
      <VoiceButton
          v-for="voice in category.voiceList" :key="voice.name"
          :value="voice.name"
          @click.native="play()"
      />


    </div>


  </div>
</template>

<script>
import VoiceButton from './components/VoiceButton.vue'
import VoiceList from '@/../setting/voices.json'


const CDN = "https://cdn.jsdelivr.net/gh/vbup-osc/mio-button@master/public/voices"

export default {
  name: 'App',
  data(){
    return{
      // category:VoiceList.voices,
      // 不能在category基础上在来一个数据嘛
      voices:VoiceList.voices
    }
  },
  components: {
     VoiceButton
  },
  methods: {
      play() {
          // todo ！！！播放逻辑
          // path 得到video路径
          // 放到里面可以在每次播放的时候灵活cdn和本地？ 为什么用const？
          // const voice = VoiceList.voices[0].voiceList[0]
          const path = `${CDN}/${this.voice.path}`
          let audio = new Audio(path)
          audio.play()
          console.log(12)
          console.log(this.voice.name)
          console.log(audio)
          
      },
      mounted() {
        console.log(VoiceList)
      },


  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
