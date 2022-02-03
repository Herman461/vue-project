<template>
  <div v-if="cardData" class="card">
    <img :src="cardData.imageSrc" alt="">
    <span>{{ cardData.number }}</span>
  </div>
</template>

<script>
export default {
  name: "v-card",
  props: {
    cardData: {type: Object}
  },
  methods: {

  }
}
</script>


<style scoped lang="scss">
.card {
  img {
    object-fit: cover;
    width: 100%;
    height: 100px;
  }
  span {
    background-color: #202020;
    border-radius: 50%;
    position: absolute;
    top: -7px;
    font-size: 12px;
    right: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 20px;
    min-width: 20px;
    padding: 1px 3px;
  }
}
</style>
