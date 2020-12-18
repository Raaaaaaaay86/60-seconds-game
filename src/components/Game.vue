<template lang="pug">
div.wrapper-game#game
  div.state
    div.state-status
      div.state-status-title
        p 60 seconds challenge
      div.state-status-score
        div
          p score
        p {{ outputScore }}
    div.state-time
      p {{ minute }}：{{ second }}
  div.question
    div.formula
      span {{ num1 }}
      span {{ operator }}
      span {{ num2 }}
      span =
      div.inputArea
        input(type="number" v-model="inputAnswer" @keydown.enter="validateAnswer()")
        small press enter to answer
  div
</template>

<script>
import {
  ref,
  computed,
  onBeforeMount,
  watchEffect,
  inject,
} from 'vue';

export default {
  setup() {
    const remainTime = ref(60);
    const minute = computed(() => Math.floor(remainTime.value / 60).toLocaleString('en-US', { minimumIntegerDigits: 2, useGrouping: false }));
    const second = computed(() => Math.floor(remainTime.value % 60).toLocaleString('en-US', { minimumIntegerDigits: 2, useGrouping: false }));

    const STEP = inject('step');
    const toResultPage = () => {
      STEP.value = 2;
    };

    // 計時
    const Counter = setInterval(() => {
      remainTime.value -= 1;
    }, 1000);

    watchEffect(() => {
      if (remainTime.value === 0) {
        clearInterval(Counter);
        toResultPage();
      }
    });

    // 產生問題
    const num1 = ref(0);
    const num2 = ref(0);
    const operator = ref('');
    const formulaOperator = ref('');
    const operatorList = ['+', '-', '*', '/'];

    const generateNewQuestion = () => {
      const newOperator = operatorList[Math.floor(Math.random() * operatorList.length)];
      let range = 9;
      let min = 0;

      if (remainTime.value >= 40 && remainTime.value <= 60) {
        range = 9 - min + 1;
      } else if (remainTime.value >= 20 && remainTime.value <= 39) {
        min = 10;
        range = 99 - min + 1;
      } else if (remainTime.value >= 0 && remainTime.value <= 19) {
        min = 100;
        range = 999 - min + 1;
      }

      num1.value = Math.round(Math.random() * range + min);
      num2.value = Math.round(Math.random() * range + min);
      formulaOperator.value = newOperator;

      switch (newOperator) {
        case '*':
          operator.value = '×';
          break;
        case '/':
          operator.value = '÷';
          break;
        default:
          operator.value = newOperator;
      }
    };

    // 驗證答案以及計分
    const inputAnswer = ref(null);
    const FINAL_SCORE = inject('finalScore');
    const outputScore = computed(() => FINAL_SCORE.value.toLocaleString('en-US', { minimumIntegerDigits: 3, useGrouping: false }));

    const validateAnswer = () => {
      const answer = eval(`${num1.value}${formulaOperator.value}${num2.value}`);

      if (parseInt(inputAnswer.value, 10) === Math.floor(answer)) {
        if (remainTime.value <= 40) {
          FINAL_SCORE.value += 5;
        } else if (remainTime.value >= 41 && remainTime.value <= 60) {
          FINAL_SCORE.value += 1;
        }
        inputAnswer.value = null;

        return generateNewQuestion();
      }

      if (FINAL_SCORE.value !== 0) FINAL_SCORE.value -= 1;
      inputAnswer.value = null;

      return generateNewQuestion();
    };

    onBeforeMount(() => {
      generateNewQuestion();
    });

    return {
      minute,
      second,
      generateNewQuestion,
      num1,
      num2,
      operator,
      validateAnswer,
      inputAnswer,
      outputScore,
    };
  },
};
</script>

<style lang="scss">
$primary: #FF9D00;
.wrapper-game {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

input[type=number] {
  -moz-appearance: textfield;
}

.state {
  text-transform: uppercase;
  font-weight: 800;
  display: flex;
  align-items: center;
  justify-content: space-between;
  &-status {
    font-size: 30px;
    margin-right: 128px;
    &-title {
      padding: 8px 16px;
      color: #fff;
      border: 4px solid #fff;
    }
    &-score {
      display: flex;
      align-items: center;
      > div {
        color: $primary;
        padding: 12px 16px;
        margin-right: 8px;
        background-color:#fff;
      }
      > p {
        color: #000;
        font-size: 44px;
      }
    }
  }
  &-time {
    color: #fff;
    font-size: 96px;
    font-style: italic;
  }
}

.formula {
  margin-top: 100px;
  color: #000;
  display: flex;
  align-items: center;
  span, input {
    font-size: 112px;
    font-weight: 800;
  }
  span:nth-of-type(even) {
    color: #fff;
    margin: 0 30px;
  }
  .inputArea {
    display: flex;
    flex-direction: column;
    align-items: center;
    input {
      width: 255px;
      height: 130px;
      margin-bottom: 8px;
      text-align: center;
      border:none;
      outline: none;
      background-color: #fff;
      &::-webkit-inner-spin-button, &::-webkit-outer-spin-button {
        -webkit-appearance: none;
        margin: 0;
      }
    }
    small {
      color: #fff;
      font-size: 24px;
      font-weight: 500;
      font-style: italic;
    }
  }
}
</style>
