<template>
  <main-screen v-if="statusGame === 'default'" @onStart="onHeadleBeforeStart($event)" />
  <inter-srcreen v-if="statusGame === 'inGame'" :cardReady="settings.cardReady" @onFinish="onGetResult" @onBack="onBack"/>
  <result-screen v-if="statusGame ==='result'" :timer="timer" @onStartAgain="statusGame ='default'"/>
  
</template>

<script>
import MainScreen from "./components/MainScreen.vue"
import InterSrcreen from "./components/InterSrcreen.vue";
import ResultScreen from "./components/ResultScreen.vue";

import {shuffled} from "./utils/Array";
export default {
  name: 'App',
  data() {
    return {
      settings: {
        totalOfBlocks: 0,
        cardReady: [],
        timeOverGame: null,
      

      },
      statusGame: "default",
      timer:0,
    };
  },  
  components: {
    MainScreen,
    InterSrcreen,
    ResultScreen,
  },
  methods:{
    onHeadleBeforeStart( config ){
      this.settings.totalOfBlocks = config.totalOfBlocks
      const firstCards = Array.from(
      {length: this.settings.totalOfBlocks / 2}, 
      (_, i)=>i+1); //Tạo mảng đầu tiên tới từ 1 tăng dần, ví dụ với length là 8 thì mảng sẽ là 1 2 3 4
      const secondCards = [...firstCards];
      const cards = [...firstCards,...secondCards];

      this.settings.cardReady = shuffled(shuffled(shuffled(shuffled(cards)))); // gán cho cardReady sau khi mix nhiều lần
      this.settings.timeOverGame = new Date().getTime();
 
      this.statusGame = 'inGame';
    },
    onGetResult(){
      this.timer = new Date().getTime() - this.settings.timeOverGame;

      this.statusGame = "result";
    },
    onBack(){
      this.statusGame = "default";
    }
  },  
}
</script>


