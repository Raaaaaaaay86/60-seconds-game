<template lang="pug">
div.wrapper-result#result
  div.resultTitle
    p 60 seconds challenge
  div.finalScore-title
    span —
    h1 YOUR FINAL SCORE
    span —
  h2.finalScore-number {{ FINAL_SCORE }}
  div(class="resetBtn" @click="toEntrance()")
    p try again!
</template>

<script>
import { inject } from 'vue';

export default {
  setup() {
    const FINAL_SCORE = inject('finalScore');

    const STEP = inject('step');
    const toEntrance = () => {
      STEP.value = 0;
      FINAL_SCORE.value = 0;
    };

    return {
      toEntrance,
      FINAL_SCORE,
    };
  },
};
</script>

<style lang="scss">
$primary: #FF9D00;

.wrapper-result {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.resultTitle {
  padding: 8px 16px;
  margin-bottom: 16px;
  color: #fff;
  font-family: 30px;
  font-weight: 800;
  border: 4px solid #fff;
  text-transform: uppercase;
  flex-shrink: 1;
}

.finalScore {
  &-title {
    display: flex;
    font-weight: 800;
    font-size: 56px;
    > h1 {
      margin: 0 16px;
    }
    > span {
      color: #fff;
    }
  }
  &-number {
    font-size: 112px;
    font-weight: 800;
    text-align: center;
    margin-bottom: 104px;
  }
}

.resetBtn {
  padding: 8px 90px;
  color: $primary;
  background-color: #000;
  font-size: 36px;
  font-weight: 800;
  text-align: center;
  cursor: pointer;
  text-transform: uppercase;
}
</style>
