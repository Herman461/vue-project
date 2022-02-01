<template>
  <div class="app">
    <div class="app__container">
      <h1 class="app__title">Containers</h1>
      <div class="app__row">
        <div class="app__column">
          <div class="app__cards app-cards">
            <div :key="card.number" class="app-cards__item" v-for="(card, index) in cards.filter(item => item.isInLeftColumn)">
              <img :src="card.imageSrc" alt="">
              <span>{{ card.number }}</span>
            </div>
          </div>
        </div>
        <div class="app__column">
          <div class="app__cards app-cards">
            <div :key="card.number" class="app-cards__item" v-for="card in cards.filter(item => !item.isInLeftColumn)">
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      totalCardsCount: 10000,
      loadedCardsCount: 0,
      cards: []
    }
  },
  mounted() {
    this.loadCards()
  },
  methods: {
    loadCards(cardsCount = 20) {
      if (this.loadedCardsCount > this.totalCardsCount) return;

      for (let i = 0; i < cardsCount; ++i) {
        const lastCardNumber = this.cards[this.cards.length - 1]?.number;

        const card = {
          imageSrc: require("./assets/img/01.jpg"),
          isInLeftColumn: true,
          number: lastCardNumber ? lastCardNumber + 1 : 1
        };

        this.cards.push(card);
      }

    }
  }
}
</script>
<style lang="scss">
@import "./assets/scss/reset.scss";
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700;900&display=swap');
body {
  background-color: #363636;
  color: #fff;
  font-family: "Roboto", sans-serif;
}
.app-cards {
  display: flex;
  flex-wrap: wrap;
  border-radius: 10px;
  padding: 25px 20px;
  background-color: #504f4f;
  min-height: 150px;
  margin: 0 -10px;
  &__item {
    flex: 0 1 25%;
    padding: 0 10px;
    margin-bottom: 20px;
    position: relative;
    img {
      object-fit: cover;
      width: 100%;
      height: 100px;
    }
    span {
      background-color: #363636;
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
}
.app {
  padding-top: 40px;
  &__container {
    margin: 0 auto;
    max-width: 1350px;
    padding: 0 15px;
  }
  &__title {
    font-size: 35px;

    font-weight: 700;
    text-align: center;
    &:not(:last-child) {
      margin-bottom: 30px;
    }
  }
  &__row {
    display: flex;
    margin: 0 -30px;
  }
  &__column {
    flex: 0 1 50%;
    padding: 0 30px;
  }
}
</style>
