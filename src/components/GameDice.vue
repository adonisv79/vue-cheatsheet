<template>
  <div class="square" @click="roll" title="Click to roll">
  {{ data.value }}
  </div>
</template>

<script lang="ts">
import { getCurrentInstance, defineComponent, reactive } from 'vue';

export default defineComponent({
  name: 'GameDice',
  props: {
    value: { type: Number, default: 0 },
  },
  setup(props, { emit }) {
    const self = getCurrentInstance();
    const data = reactive({
      value: 1,
    });

    const roll = () => {
      data.value = Math.floor(Math.random() * 6) + 1;
      emit('rolled', self?.vnode.key, data.value);
    };

    return {
      data,
      roll,
    };
  },
});
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css?family=Bangers');

.square {user-select: none;
  font-family: 'Bangers', sans-serif;
  font-size: 2em;
  line-height: 47px;
  height: 47px;
  width: 47px;
  vertical-align: middle;
  background-color: red;
  color: white;
  display: inline-block;
  border-radius: 5px;
  border-color:tomato;
  border-width: 3px;
  border-style: outset;
}
</style>
