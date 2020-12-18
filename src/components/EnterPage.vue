<template lang="pug">
div.wrapper-enterPage#entrance
  div.title
    p 60
    div
      div
        p seconds
        p +−×÷
      p challenge
  div
    div(class="startBtn" @click="toGame()") start!
  p.description try to answer more as you can
</template>

<script>
import { inject } from 'vue';

export default {
  setup() {
    const STEP = inject('step');

    const toGame = () => {
      STEP.value = 1;
    };

    return {
      toGame,
    };
  },
};
</script>

<style lang="scss">
$primary: #FF9D00;

.wrapper-enterPage {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.title {
  display: flex;
  justify-content: center;
  align-items: center;
  > p {
    margin-right: 24px;
    font-size: 240px;
    color: #fff;
    text-shadow: 12px 0 0 #000;
  }
  > div {
    display: flex;
    flex-direction: column;
    color: #000;
    font-size: 96px;
    font-weight: 800;
    text-transform: uppercase;
    > div{
      display: flex;
      align-items: center;
      justify-content: space-between;
      p:last-of-type {
        position: relative;
        color: #fff;
        font-size: 44px;
        border-bottom: 4px solid #fff;
        &::after {
          content: '';
          width: 100%;
          position: absolute;
          bottom: -12px;
          left: 0;
          border-bottom: 4px solid #fff;
        }
      }
    }
  }
}

.startBtn {
  margin-top: 104px;
  padding: 8px 122px;
  background-color: #000;
  color: $primary;
  font-size: 36px;
  text-align: center;
  font-weight: 800;
  cursor: pointer;
  text-transform: uppercase;
}

.description {
  margin-top: 8px;
  color: #fff;
  font-size: 24px;
  font-style: italic;
}
</style>
