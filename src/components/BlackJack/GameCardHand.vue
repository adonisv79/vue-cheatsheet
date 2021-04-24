<template>
  <section class="game-card-hand">
    <p>Cards in hand:</p>
    <game-card v-for="card in data.cardsInHand" :key="card" :value="card" :startFacedDown="false" />
  </section>
</template>

<script lang="ts">
import { defineComponent, reactive } from 'vue';
import GameCard from './GameCard.vue';

export default defineComponent({
  components: { GameCard },
  props: {
    maxHandSize: {
      type: Number,
      default: 10,
    },
  },
  setup(props) {
    const data = reactive<any>({
      cardsInHand: [],
    });

    const addCard = (value: string) => {
      if (data.cardsInHand.length < props.maxHandSize) data.cardsInHand.push(value);
      else throw new Error('Max hand reached');
    };

    const isMaxHandReached = () => data.cardsInHand.length >= props.maxHandSize;

    const restartHand = () => {
      data.cardsInHand = [];
    };

    return {
      data,
      isMaxHandReached,
      addCard,
      restartHand,
    };
  },
});
</script>

<style scoped>
.game-card-hand {
  color: white;
  background-color: #111;
  min-height: 142px;
}
</style>
