<template>
  <div
      class="tooltip"
      ref="tooltip"
      :style="{ top: coords.top + 'px', left: coords.left + 'px'}"
  >
    <div class="tooltip__body"
         v-if="isShown"

    >
      <v-card :card-data="cardData"/>
    </div>

  </div>
</template>

<script>
import VCard from "./Card";

export default {
  name: "v-tooltip",
  components: {VCard},
  // props: {
  //   cardData: {type: Object}
  // },
  data() {
    return {
      isShown: false,
      coords: {
        left: 0,
        top: 0
      },
      cardData: {}
    }
  },
  methods: {
    show(cardData) {
      this.isShown = true
      this.cardData = cardData
    },
    hide() {
      this.isShown = false
    },
    setPosition(target) {
      const coords = target.getBoundingClientRect()

      this.coords.left = coords.left + (target.offsetWidth - this.$refs.tooltip.offsetWidth) / 2;
      if (this.coords.left < 0) this.coords.left = 15;

      this.coords.top = coords.top - this.$refs.tooltip.offsetHeight - 25
      if (this.coords.top < 0) {
        this.coords.top = coords.top + target.offsetHeight + 25;
      }
    }
  }
}
</script>


<style scoped lang="scss">
.tooltip {
  position: fixed;
  width: 400px;
  pointer-events: none;
  height: 300px;
  &__body {
    position: relative;
    border: 3px solid #fff;
    height: 100%;
    &::after {
      content: '';
      position: absolute;
      left: 50%;
      transform: translate(-50%, 0);
      top: -42px;
      border: 20px solid transparent;
      border-bottom: 20px solid #fff;
    }
  }
}
</style>

<style lang="scss">
.tooltip {
  .card {
    height: 100%;
    img {
      height: 100%;
    }
    span {
      width: 30px;
      height: 30px;
      font-size: 18px;
      top: -12px;
      right: -7px;
    }
  }
}
</style>
