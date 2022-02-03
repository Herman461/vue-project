<template>
  <div class="app">
    <div class="app__container">
      <h1 class="app__title">Containers</h1>
      <div class="app__row">
        <div class="app__column">
          <button @click="sortLeftColumnCards" type="button" class="app__sort-btn">Sort</button>
          <div ref="column" @scroll="handleScroll" class="app__cards app-cards">
            <ul class="app-cards__list">
              <li
                  @mouseenter="showTooltip(card); setPosition($event);"
                  @mouseleave="hideTooltip"
                  @click="addToRightColumn(card, index)"
                  :key="card.number" class="app-cards__item"
                  v-for="(card, index) in leftColumnCards"
              >
                <v-card :card-data="card" />
              </li>
            </ul>
          </div>
        </div>
        <div class="app__column">
          <button @click="sortRightColumnCards" type="button" class="app__sort-btn">Sort</button>
          <div class="app__cards app-cards">
            <ul class="app-cards__list">
              <li
                  @mouseenter="showTooltip(card); setPosition($event);"
                  @mouseleave="hideTooltip"
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
      totalCardsCount: 100,
      loadedCardsCount: 0,
      leftColumnCards: [],
      rightColumnCards: []
    }
  },
  mounted() {
    if (this.$refs.column.clientHeight === this.$refs.column.scrollHeight) {
      this.loadCards(40)
    }
  },
  methods: {
    sortRightColumnCards(cards) {
      this.rightColumnCards.sort((a, b) => a.number - b.number)
    },
    sortLeftColumnCards(cards) {
      this.leftColumnCards.sort((a, b) => a.number - b.number)
    },
    handleScroll(e) {
      if (this.$refs.column.scrollTop + this.$refs.column.clientHeight > this.$refs.column.scrollHeight - 120) {
        this.loadCards()
      }
    },
    showTooltip(cardData) {
      this.$refs.tooltip.show(cardData)
    },
    hideTooltip() {
      this.$refs.tooltip.hide()
    },
    loadCards(cardsCount = 20) {
      if (this.loadedCardsCount >= this.totalCardsCount) return

      this.loadedCardsCount += cardsCount

      for (let i = 0; i < cardsCount; ++i) {
        const lastCardNumber = this.leftColumnCards[this.leftColumnCards.length - 1]?.number

        const card = {
          imageSrc: require("./assets/img/01.jpg"),
          number: lastCardNumber ? lastCardNumber + 1 : 1
        };

        this.leftColumnCards.push(card)
      }

    },
    addToLeftColumn(card, cardIndex) {
      this.hideTooltip()
      this.rightColumnCards.splice(cardIndex, 1);
      this.leftColumnCards.push(card);
    },
    addToRightColumn(card, cardIndex) {
      this.hideTooltip()
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

  background-color: #504f4f;
  overflow: auto;
  border-radius: 10px;
  min-height: calc(100vh - 195px);
  max-height: calc(100vh - 195px);
  &__list {
    display: flex;
    flex-wrap: wrap;
    padding: 25px 20px;
  }
  &__item {
    height: 100px;
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
  &__sort-btn {
    font-size: 18px;
    letter-spacing: 0.025em;
    color: #fff;
    display: inline-flex;
    justify-content: center;
    align-items: center;
    min-height: 45px;
    font-weight: 700;
    padding-left: 35px;
    padding-right: 35px;
    background-color: #504f4f;
    border-radius: 4px;
    transition: all 0.3s ease 0s;
    &:not(:last-child) {
      margin-bottom: 25px;
    }
    @media (any-hover: hover) {
      &:hover {
        background-color: #fff;
        color: #504f4f;
      }

    }
  }
}
</style>
