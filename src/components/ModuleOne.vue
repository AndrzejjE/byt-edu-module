<template>
  <div id="app">
    <div class="container">
      <div class="question">
        <p>({{number1}})</p>
        <span v-for="dot in number1" :key="dot" class="dot"></span>
        <span class="comparison">[?]</span>
        <span v-for="dot in number2" :key="dot" class="dot"></span>
        <p>({{number2}})</p>
      </div>
      <div class="buttons">
        <button @click="checkAnswer(true)">&gt;</button>
        <button @click="checkAnswer('equal')">=</button>
        <button @click="checkAnswer(false)">&lt;</button>
      </div>
      <div v-if="showResult" class="result">
        <p>{{ resultMessage }}</p>
        <button @click="nextQuestion">Następne pytanie</button>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive, toRefs } from "vue";

export default {
  name: 'EduM',
  setup() {
    const state = reactive({
      number1: 5,
      number2: 2,
      showResult: false,
      resultMessage: ''
    });

    const checkAnswer = (selectedAnswer) => {
      state.showResult = true;
      let isCorrect;

      if (selectedAnswer === 'equal') {
        isCorrect = state.number1 === state.number2;
      } else {
        isCorrect = selectedAnswer === (state.number1 > state.number2);
      }

      if (isCorrect) {
        state.resultMessage = 'Dobra odpowiedź!';
      } else {
        state.resultMessage = 'Zła odpowiedź.';
      }
    };

    const nextQuestion = () => {
      state.number1 = Math.floor(Math.random() * 10) + 1;
      state.number2 = Math.floor(Math.random() * 10) + 1;
      state.showResult = false;
      state.resultMessage = '';
    };

    return {...toRefs(state), checkAnswer, nextQuestion};
  }
};
</script>

<style scoped>
#app {
  font-family: 'Arial', sans-serif;
  text-align: center;
  margin: 0 auto;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
}

.container {
  max-width: 400px;
  margin: 0 auto;
  border: 1px solid #ccc;
  padding: 20px;
  border-radius: 8px;
}

.question {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 20px;
}

.dot {
  width: 10px;
  height: 10px;
  background-color: #3498db;
  border-radius: 50%;
  margin: 0 5px;
  display: inline-block;
}

.comparison {
  font-size: 18px;
  margin: 0 10px;
}

.buttons {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
}

.buttons button {
  background-color: #2ecc71;
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.result {
  margin-top: 20px;
}

.result p {
  font-size: 18px;
  margin-bottom: 10px;
}

.result button {
  background-color: #3498db;
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
</style>
