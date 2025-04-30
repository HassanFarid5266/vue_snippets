<template>
  <div class="app">
    <p class="victoryState" v-if="victory">Done! <button v-on:click="cardsShuffle" class="btn btn-primary">Refresh
        ?</button></p>
    <div v-for="(card, index) in cards" :key="index"
      :class="[{ 'down': card.down && !card.matched, 'up': !card.down, 'matched': card.matched }, ' card']"
      v-on:click="handleClick(card)">
      <i :class="'fas ' + card.icon"></i>
    </div>
  </div>
</template>

<script>
import _ from 'lodash';

export default {
  data() {
    return {
      cards: _.range(0, 24), // assuming 12 icons * 2
      runing: false,
    };
  },
  mounted() {
    this.cardsShuffle();
  },
  methods: {
    cardsShuffle() {
      const icons = [
        "fa-kiwi-bird",
        "fa-chess",
        "fa-frog",
        "fa-camera-retro",
        "fa-plug",
        "fa-anchor",
        "fa-birthday-cake",
        "fa-cube",
        "fa-dice",
        "fa-bug",
        "fa-cut",
        "fa-gem"
      ];

      this.cards = _.shuffle([...icons, ...icons].map(icon => ({
        icon,
        down: true,
        matched: false
      })));
    },
    handleClick(cardClicked) {
      if (!this.runing) {
        if (!cardClicked.matched && this.cardCount.cardsUp < 2) {
          cardClicked.down = false;
        }
        if (this.cardCount.cardsUp === 2) {
          this.runing = true;
          setTimeout(() => {
            let match = this.checkMatch(this.cardCount.icons);
            this.cards.forEach(card => {
              if (match && !card.down && !card.matched) {
                card.matched = true;
              } else {
                card.down = true;
              }
            });
            this.runing = false;
          }, 1500);
        }
      }
    },
    checkMatch(icons) {
      return icons[0] === icons[1];
    }
  },
  computed: {
    cardCount() {
      let cardUpCount = 0;
      let cardMatchedCount = 0;
      let icons = [];
      this.cards.forEach((card) => {
        if (!card.down && !card.matched) {
          cardUpCount++;
          icons.push(card.icon);
        }
        if (card.matched) {
          cardMatchedCount++;
        }
      });
      return {
        cardsUp: cardUpCount,
        cardsMatched: cardMatchedCount,
        icons: icons
      };
    },
    victory() {
      return this.cardCount.cardsMatched === this.cards.length;
    }
  }
};
</script>


<style scoped lang="scss">
body {
  background: oldlace;
  padding: 20px;
  font-family: Helvetica, Arial, Sans-Serif;
}

.app {
  display: grid;
  grid-template-columns: repeat(auto-fit, 100px);
  grid-auto-rows: 100px;
  grid-gap: 15px;
  justify-content: center;
  perspective: 800px;
  max-width: 720px;
  margin: 0 auto;
}

i.fas {
  font-size: 40px;
  color: #333;
}

.victoryState {
  grid-column-start: 1;
  grid-column-end: -1;
  text-align: center;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.card {
  background: #fff;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 5px;
  box-shadow: 5px 5px 0 0 #333;
  cursor: pointer;
  animation: flipUp .5s forwards;

  i {
    opacity: 1;
    transition: opacity .5s;
  }

  &.down {
    animation: flipDown .5s forwards;

    i {
      opacity: 0;
    }
  }

  &.matched {
    animation: matching .3s forwards;
  }
}

@keyframes flipDown {
  0% {
    background: #fff;
    transform: rotateY(0deg);
    box-shadow: 5px 5px 0 0 #333;
  }

  100% {
    background: rgb(115, 132, 127);
    transform: rotateY(180deg);
    box-shadow: -5px 5px 0 0 #333;
  }
}

@keyframes flipUp {
  0% {
    background: rgb(115, 132, 127);
    transform: rotateY(180deg);
    box-shadow: -5px 5px 0 0 #333;
  }

  100% {
    background: #fff;
    transform: rotateY(0deg);
    box-shadow: 5px 5px 0 0 #333;
  }
}

@keyframes matching {
  0% {
    background: #fff;
  }

  100% {
    background: lightpink;
  }
}
</style>