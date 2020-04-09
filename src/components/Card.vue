<script>
export default {
  props: {
    backFace: String,
    isStart: Boolean,
    bus: Object,
    isSelect: Boolean,

},
  data() {
    return {
      isClick: false,
      result: String,
    };
  },

  methods: {
    showQueen() {
      if (this.backFace == "queen.png") {
        this.isClick = true;
        setTimeout(() => {
          this.isClick = false;
        }, 3000);
      }
    },

    changeFace() {
      if (!this.isStart && !this.isSelect) {
        if (this.backFace == "queen.png")
          this.result = "Congratulations";
        else  this.result = "Try Again";
        this.bus.$emit("result", this.result);
        this.isClick = true;
        setTimeout(() => {
          this.isClick = false;
        }, 3000);
      }
    },
  },

  created() {
    this.bus.$on("level", () => {
      this.showQueen();
    });
  },
};
</script>

<template>
  <div class="card" :class="{noHover : isStart}" @click="changeFace">
    <div class="card__face card__face--front" :class="{ showFront: isClick }">
      <img src="@/assets/card-front.png" />
    </div>
    <div class="card__face card__face--back" :class="{ showBack: isClick }">
      <img :src="require(`../assets/${backFace}`)" />
    </div>
  </div>
</template>

<style lang="scss">
$breakpoint-tablet: 768px;
.card {
  width: 200px;
  height: 260px;
  position: relative;
  transition: transform 1s;
  transform-style: preserve-3d;
  margin-right: 30px;
  @media (max-width: $breakpoint-tablet) {
    width: 75px;
    margin-top: 40px;
  }
}

img {
  width: inherit;
}
/*.card { 
  //transform-origin: center right; 
}

.card:hover > .card__face--front{
    transform: rotateY( -180deg );

}
.card:hover > .card__face--back{
    transform: rotateY( 0deg );

}*/

.showFront {
  transform: rotateY(-180deg) !important;
}

.showBack {
  transform: rotateY(0deg) !important;
}
.card:hover {
  transform: scale(1.1);
}

.noHover:hover{
  transform: scale(1);
}

.card.is-flipped {
  transform: translateX(-100%) rotateY(-180deg);
}

.card__face {
  position: absolute;
  height: 100%;
  width: 100%;
  backface-visibility: hidden;
  transition: all 1s ease;
}

.card__face--back {
  transform: rotateY(180deg);
}

.card.is-flipped {
  transform: rotateY(180deg);
}
</style>
