<template>
  <section class='game-dice-pool'>
    <section class="title">{{ title }}</section>
    <button v-if="allowAddRemove" @click="removeDice"
      title='Removes a new dice in the pool'>-</button>
    <button v-if="allowAddRemove" @click="addDice"
      title='Adds a new dice to the pool'>+</button>
    <button @click="rollAllDice"
      title='Rolls all the dice in the pool'>Roll All</button>
    Result: {{ data.totalValue }}
    <div class="dice-area">
      <game-dice v-for="dice in data.dices" :key="dice.id" @rolled="onDiceRolled"
        :ref="setDiceRefs"></game-dice>
    </div>
  </section>
</template>

<script lang="ts">
import {
  onBeforeUpdate, defineComponent, reactive, onBeforeMount,
} from 'vue';
import GameDice from './GameDice.vue';

interface DiceItem {
  id: number,
  value: number,
}
interface DiceGroupData {
  totalValue: number,
  dices: DiceItem[]
}

export default defineComponent({
  components: { GameDice },
  props: {
    title: {
      type: String,
      required: true,
    },
    allowAddRemove: {
      type: Boolean,
      default: false,
    },
    maxDice: {
      type: Number,
      default: 3,
      validator: (value: number) => {
        if (value < 10) return true;
        return false;
      },
    },
    /**
     * Sets the minimum value
     */
    minDice: {
      type: Number,
      default: 1,
      validator: (value: number) => {
        if (value > 0) return true;
        return false;
      },
    },
  },
  name: 'GameDicePool',
  setup(props, { emit }) {
    let diceRefs: any[] = [];
    const data = reactive<DiceGroupData>({
      totalValue: 0,
      dices: [],
    });

    onBeforeMount(() => {
      for (let i = 1; i <= props.minDice; i += 1) {
        data.dices.push({ id: data.dices.length, value: 1 });
        data.totalValue += 1;
      }
    });

    onBeforeUpdate(() => {
      diceRefs = []; // reset every update
    });

    const setDiceRefs = (el: any) => {
      if (el) { diceRefs.push(el); }
    };

    /**
     * Adds a new dice to the pool and give it an index value based on previous array length
     * Does not go beyond the maximum dice count
     */
    const addDice = () => {
      if (data.dices.length < props.maxDice) data.dices.push({ id: data.dices.length, value: 1 });
    };

    /**
     * Removes the last dice item
     * Does not fall below the minimum dice count
     */
    const removeDice = () => {
      if (data.dices.length > props.minDice) data.dices.pop();
    };

    /**
     * Triggers the roll functionality on all the dices
     */
    const rollAllDice = () => {
      diceRefs.forEach((dice) => {
        dice.roll();
      });
      emit('dices-rolled', data.totalValue);
    };

    /**
     * Event handler for when a dice is rolled
     */
    const onDiceRolled = (index: number, result: number) => {
      data.dices[index].value = result;
      data.totalValue = 0;
      diceRefs.forEach((dice) => {
        data.totalValue += dice.data.value;
      });
    };

    return {
      data,
      addDice,
      removeDice,
      setDiceRefs,
      rollAllDice,
      onDiceRolled,
    };
  },
});
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css?family=Bangers');
.game-dice-pool{
  display:table-cell;
  border: 1px solid red;
  min-width: 150px;
  max-width: 220px;
  padding: 2px;
}
.game-dice-pool .dice-area {
  background-color: #ddd;
  padding: 5px;
}
.game-dice-pool .title {
  font-family: 'Bangers', sans-serif;
  background-color: red;
  color: white;

  margin: 2px;
}
</style>
