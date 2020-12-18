<template lang="pug">
EnterPage(v-if="step === 0")
Game(v-if="step === 1")
Result(v-if="step === 2")
</template>

<script>
import EnterPage from '@/components/EnterPage.vue';
import Game from '@/components/Game.vue';
import Result from '@/components/Result.vue';
import { ref, provide } from 'vue';

export default {
  name: 'App',
  components: {
    EnterPage,
    Result,
    Game,
  },
  setup() {
    const step = ref(0);
    const finalScore = ref(0);

    provide('finalScore', finalScore);
    provide('step', step);

    return {
      step,
    };
  },
};
</script>

<style lang="scss">
$primary: #FF9D00;

#app {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: $primary;
  font-family: 'Roboto Condensed', Arial, Helvetica, sans-serif;
  user-select: none;
}
</style>
