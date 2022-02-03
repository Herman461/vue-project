<template>
  <div class="app">
    <div class="app__container">
      <h1 class="app__title">Containers</h1>
      <div class="app__row">
        <div class="app__column">
          <ul class="app__cards app-cards">
            <li
                @mouseover="showTooltip(card); setPosition($event);"
                @mouseout="hideTooltip"
                @click="addToRightColumn(card, index)"
                :key="card.number" class="app-cards__item"
                v-for="(card, index) in leftColumnCards"
            >
              <v-card :card-data="card" />
            </li>
          </ul>
        </div>
        <div class="app__column">
          <ul class="app__cards app-cards">
            <li
                @click="addToLeftColumn(card, index)"
                :key="card.number"
                class="app-cards__item"
                v-for="(card, index) in rightColumnCards"
            >
              <v-card :card-data="card" />
            </li>
          </ul>
        </div>
      </div>
    </div>
    <v-tooltip ref="tooltip" />
  </div>
</template>

<script>
import VTooltip from "./views/Tooltip";
import VCard from "./views/Card";
export default {
  name: "app",
  components: {VCard, VTooltip},
  data() {
    return {
      totalCardsCount: 10000,
      loadedCardsCount: 0,
      leftColumnCards: [],
      rightColumnCards: []
    }
  },
  mounted() {
    this.loadCards()
  },
  methods: {
    showTooltip(cardData) {
      this.$refs.tooltip.show(cardData)
    },
    hideTooltip() {
      this.$refs.tooltip.hide()
    },
    loadCards(cardsCount = 20) {
      if (this.loadedCardsCount > this.totalCardsCount) return;

      for (let i = 0; i < cardsCount; ++i) {
        const lastCardNumber = this.leftColumnCards[this.leftColumnCards.length - 1]?.number;

        const card = {
          imageSrc: require("./assets/img/01.jpg"),
          number: lastCardNumber ? lastCardNumber + 1 : 1
        };

        this.leftColumnCards.push(card);
      }

    },
    addToLeftColumn(card, cardIndex) {
      this.rightColumnCards.splice(cardIndex, 1);
      this.leftColumnCards.push(card);
    },
    addToRightColumn(card, cardIndex) {
      this.leftColumnCards.splice(cardIndex, 1);
      this.rightColumnCards.push(card);
    },
    setPosition(event) {
      this.$refs.tooltip.setPosition(event.target)
    }
  },
  computed: {

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
    cursor: pointer;
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
