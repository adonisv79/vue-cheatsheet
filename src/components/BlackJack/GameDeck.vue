<template>
  <section class="game-deck">
    <game-card value="KK"></game-card>
    <span>
     <img class="game-card" src="../../assets/card-back.png" />
    </span>
    <p>{{ data.deck.length }} cards</p>
  </section>
</template>

<script lang="ts">
import { defineComponent, onBeforeMount, reactive } from 'vue';

export default defineComponent({
  setup(props, { emit }) {
    const cardTypes = [
      'A♠', 'K♠', 'Q♠', 'J♠', '10♠', '9♠', '8♠', '7♠', '6♠', '5♠', '4♠', '3♠', '2♠',
      'A♦', 'K♦', 'Q♦', 'J♦', '10♦', '9♦', '8♦', '7♦', '6♦', '5♦', '4♦', '3♦', '2♦',
      'A♥', 'K♥', 'Q♥', 'J♥', '10♥', '9♥', '8♥', '7♥', '6♥', '5♥', '4♥', '3♥', '2♥',
      'A♣', 'K♣', 'Q♣', 'J♣', '10♣', '9♣', '8♣', '7♣', '6♣', '5♣', '4♣', '3♣', '2♣',
    ];
    const data = reactive<any>({
      deck: [],
    });

    const restartDeck = () => {
      data.deck = [];
      const added: boolean[] = [];
      while (data.deck.length < 52) {
        const cardTypeindex = Math.floor(Math.random() * 52);
        if (!added[cardTypeindex]) {
          data.deck.push(cardTypes[cardTypeindex]);
          added[cardTypeindex] = true;
        }
      }
    };

    const drawCards = (count = 1) => {
      const drawnCards = [];
      for (let i = 1; i <= count; i += 1) {
        drawnCards.push(data.deck.pop());
      }
      emit('cardsDrawn', drawnCards);
    };

    onBeforeMount(() => {
      restartDeck();
    });

    return {
      data,
      drawCards,
      restartDeck,
    };
  },
});
</script>

<style scoped>
  .game-deck {
    user-select: none;
    width: 67px;
    border: solid 1px black;
  }
  .game-deck span {
    display: flex;
    width: 67px;
  }
  .game-deck img {
    width: 67px;
    height: 100px;
    border: 1px solid black;
    background-color: #eee;
    border-radius: 5px;
  }
</style>
