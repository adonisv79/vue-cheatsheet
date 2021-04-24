<template>
  <section class="game-black-jack">
    <button @click="drawFromDeck">Draw a card</button>
    <button @click="restartGame">Start Again</button>
    <span>
      <game-deck @cards-drawn="onCardsDrawn" ref="deckComponent" />
      <game-card-hand :maxHandSize="3" ref="playeHandComponent" />
    </span>
  </section>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import GameDeck from './GameDeck.vue';
import GameCardHand from './GameCardHand.vue';

export default defineComponent({
  components: { GameDeck, GameCardHand },
  setup() {
    const deckComponent = ref(GameDeck);
    const playeHandComponent = ref(GameCardHand);
    const onCardsDrawn = (cards: string[]) => {
      playeHandComponent.value.addCard(cards[0]);
      console.log(cards[0]);
    };

    const drawFromDeck = () => {
      if (!playeHandComponent.value.isMaxHandReached()) {
        deckComponent.value.drawCards(1);
      } else {
        alert('Max hand is reached');
      }
    };

    const restartGame = () => {
      deckComponent.value.restartDeck();
      playeHandComponent.value.restartHand();
    };

    return {
      drawFromDeck,
      deckComponent,
      playeHandComponent,
      onCardsDrawn,
      restartGame,
    };
  },
});
</script>

<style scoped>
.game-black-jack {
  background-color: green;
  width: 500px;
  border: solid 10px brown;
  border-style: outset;
  padding: 10px;
}
</style>
