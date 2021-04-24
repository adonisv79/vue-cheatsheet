<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <h1>Dice Test</h1>
  <p>In this sample, we use the Vue3 Composition-api and explore
    The most common tools we need for building a VueJS app.
    The template for creating a component, setting props, importing another component,
    creating reactive data, exposing functionality, emiting and listening to events,
    referencing v-for elements using functions and more...
  </p>
  <game-dice-pool title="5 dices" :minDice="5"></game-dice-pool>
  <game-dice-pool title="Add and Remove Dices"
    :allowAddRemove="true" :minDice="2" :maxDice="8"></game-dice-pool>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import GameDicePool from './components/GameDicePool.vue';

export default defineComponent({
  components: { GameDicePool },
  setup() {
    return {};
  },
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
