<template>
    <div>
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
// 更改组件的组织方式，路径很大可能改变
    import VoiceButton from './VoiceButton.vue'
    // @是什么 src
    // https://stackoverflow.com/questions/42749973/what-does-the-mean-inside-an-import-path
    import VoiceList from '@/../setting/voices.json'
    import FormalCard from './FormalCard.vue'
    import Control from './Control.vue'
    const CDN = "https://cdn.jsdelivr.net/gh/vbup-osc/mio-button@master/public/voices"

    export default {
        data(){
            return{
            // category:VoiceList.voices,
            // 不能在category基础上在来一个数据嘛
            voices:VoiceList.voices,
            // nowPlay是一个对象，存放正在播放的voiceItem
            nowPlay:{},
            // 放弃了，加一个播放列表存储每个正在播放的音频 播放完的清除
            playerList: new Array(),
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
                // 单次播放时
                if(!this.overlap){
                    // 第一次播放，playerlist里还没有值
                    // 当有once这个key后暂停它
                    // 但不是第一次的情况下里面有once值就停止播放
                    // 但是之前还可能有key是时间戳的情况
                    // 此时之前在播放列表里的音声还会播放
                    // 需要取消掉
                    this.stopPlay()
                }
                // todo ！！！播放逻辑
                // path 得到video路径
                // 放到里面可以在每次播放的时候灵活cdn和本地？ 为什么用const？
                // const voice = VoiceList.voices[0].voiceList[0]

                // 1.创建audio并随事件放入播放列表 以后好暂停
                // const key = new Date().getTime()
                const path = `${CDN}/${voice.path}`
                let audio = new Audio(path)
                // 2.播放
                audio.play()
                // 推到表里面的目的是之后统一暂停，所以在花括号结束前放入就行
                // audio声明后到下花括号结束前 位置随便放
                this.playerList.push(audio)
                // 3.展示区
                this.nowPlay = voice
                // 4.播放结束后的善后
                // 播放时就约定好 播放结束后清空展示区 当前音频离开播放列表 剩下空值，使得索引不变
                audio.onended = () => {
                    this.nowPlay = {}
                    // 不是按顺序删除，是结束时删除列表中的当前audio 所以不能pop
                    // 特定当前audio
                    // 当前的audio是最后压进去的 所以最后一个索引是当前的audio
                    // todo 但是问题来了：使用splice删除之后索引会变
                    // 使用delete索引不变
                    // playerList.length-1是当前音频的索引 随着音频的推入length不断变大
                    // 加上索引的位置不变，所以每个audio有唯一不变的索引
                    delete this.playerList[this.playerList.length-1]
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
                
                this.playerList.forEach(audio => audio.pause())

                // 2.清空展示区和播放列表
                this.nowPlay = 'null'
                // map有clear array有splice
                this.playerList.splice(0)
            },
            changeOverlap(){
                this.overlap = !this.overlap
            }


        }
    }
</script>

<style lang="scss" scoped>
    // 播放区
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