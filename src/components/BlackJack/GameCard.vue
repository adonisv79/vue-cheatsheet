<template>
  <section @click="toggle" class="game-card" title="click to flip">
    <img class="game-card" v-if="data.isFacedDown"
     src="../../assets/card-back.png" />
     <span :class="{ 'red': isHeartOrDiamond(value) }" v-if="!data.isFacedDown">{{ value }}</span>
  </section>
</template>

<script>
import { defineComponent, reactive } from 'vue';

export default defineComponent({
  props: {
    startFacedDown: {
      type: Boolean,
      default: true,
    },
    value: {
      type: String,
      required: true,
    },
  },
  setup(props) {
    const data = reactive({
      isFacedDown: props.startFacedDown,
    });

    const toggle = () => {
      data.isFacedDown = !(data.isFacedDown);
    };

    const isHeartOrDiamond = (value) => {
      if (value.indexOf('♥') >= 0 || value.indexOf('♦') >= 0) return true;
      return false;
    };

    return {
      toggle,
      data,
      isHeartOrDiamond,
    };
  },
});
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css?family=Sawarabi+Mincho');
.game-card {
  user-select: none;
  width: 67px;
  height: 100px;
  border: 1px solid black;
  background-color: #eee;
  border-radius: 5px;
  display: inline-flex;
  justify-content: center;
  align-items: center;
  color: black;
}
.game-card span {
  font-family: 'Sawarabi Mincho', sans-serif;
  font-weight: bold;
  font-size: 1.5em;
  position: absolute;
}
.red {
  color: red;
}
</style>
