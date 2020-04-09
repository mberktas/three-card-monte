<template>
  <div id="app">
    <h1>Find The Lady</h1>
    <h2>{{ result }}</h2>
    <div class="difficulty-wrapper">
      <div class="difficulty-btn" @click="level(1000)">Easy</div>
      <div class="difficulty-btn" @click="level(750)">Normal</div>
      <div class="difficulty-btn" @click="level(500)">Hard</div>
      <div class="difficulty-btn" @click="level(200)">Extreme</div>
    </div>

    <div class="scene">
      <transition-group :name="shuffleSpeed" tag="div" class="scene">
        <card
          :isSelect="isSelect"
          :backFace="card"
          :isStart="isStart"
          :bus="bus"
          v-for="card in this.cards"
          :key="card"
        >
        </card>
      </transition-group>
    </div>
  </div>
</template>

<script>
import Card from "./components/Card.vue";
import Vue from "vue";
export default {
  name: "App",
  data() {
    return {
      cards: ["queen.png", "joker.png", "king.png"],
      shuffleSpeed: "shuffleMedium",
      speed: 1000,
      isStart: false,
      bus: new Vue(),
      result: "",
      isSelect: false
    };
  },
  components: {
    Card,
  },
  methods: {
    level(difficulty) {
      this.isSelect = false;
      this.isStart = true;
      this.bus.$emit("level");
      switch (difficulty) {
        case 250:
          this.shuffleSpeed = "shuffleFastest";
          break;
        case 500:
          this.shuffleSpeed = "shuffleFast";
          break;
        case 750:
          this.shuffleSpeed = "shuffleMedium";
          break;
        case 1000:
          this.shuffleSpeed = "shuffleSlow";
          break;
      }
      var myvar;
      setTimeout(() => {
        myvar = setInterval(() => this.shuffleCards(this.cards), difficulty);
      }, 3000);
      setTimeout(() => {
        this.isStart = false
        clearInterval(myvar);
      }, 8000);
    },
    shuffleCards(array) {
      for (let i = array.length - 1; i > 0; i--) {
        let j = Math.floor(Math.random() * i); // random index from 0 to i
        //[array[i], array[j]] = [array[j], array[i]]
        let temp = this.cards[i];
        Vue.set(this.cards, i, this.cards[j]);
        Vue.set(this.cards, j, temp);
      }
      console.log(this.cards);
    },
  },
  created() {
    this.bus.$emit("level");
    this.bus.$on("result", (result) => {
        this.result = result;
        this.isSelect = true;
    });
  },
};
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Open+Sans:wght@400;700&display=swap');

$breakpoint-tablet : 768px;

body {
  background-color: #005336;
  font-family: 'Open Sans', sans-serif;
  color: #f6fdff;
  text-transform: uppercase;
  font-size: 32px;

  @media(max-width: $breakpoint-tablet){
    font-size: 24px;
  }
}
h1 {
  font-weight: bold;
  font-size: 2.3rem;
  margin-top: 100px;
  text-align: center;
  

}

h2 {
  font-size: 1.5rem;
  font-weight: bold;
  height: 15px;
  text-align: center;
  margin-bottom: 44px;
}

.difficulty-wrapper {
  display: flex;
  justify-content: center;

  @media(max-width:$breakpoint-tablet){
    justify-content: flex-start;
  }

  :nth-child(1){
    margin-left: 0;
  }
}
.difficulty-btn {
  font-weight: 400;
  background-color: #588D99;
  width: 120px;
  height: 50px;
  line-height: 50px;
  text-align: center;
  border-radius: 5px;
  cursor: pointer;
  font-size: 0.8rem;
  margin-left: 15px;
  box-shadow: 0px 0px 10px 2px rgba(88, 141, 153, 0.2);
  transition: background-color .3s linear  ;
  &:hover{
    background-color: #557777;
  }
}

.scene {
  display: flex;
  position: absolute;
  width: 100%;
  height: 100px;
  justify-content: center;
  perspective: 600px;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);

  :nth-last-child(1){
    margin-right: 0;
  }
}
// Transitions
.shuffleSlow-move {
  transition: transform 1s;
}
.shuffleMedium-move {
  transition: transform 0.75s;
}
.shuffleFast-move {
  transition: transform 0.5s;
}

.shuffleFastest-move {
  transition: transform 0.2s;
}
</style>
