<template>
  <div class="Page">
    <div v-if="level === 0" class="game-area">
      <div>
        <button v-for="lvl in 3" :key="lvl" @click="selectLevel(lvl)" class="level-button">
          Poziom {{ lvl }}
        </button>
      </div>
    </div>
    <div v-else class="game-area">
      <div class="question-counter">
        <p>Pytanie: {{ questionNumber }} / 10</p>
      </div>
      <div class="score">
        <p>Poprawne odpowiedzi: {{ correctAnswers }}</p>
      </div>
      <div class="question">
        <template v-if="level === 1">
          <span v-for="dot in number1" :key="dot" class="dot"></span>
          <span class="comparison">[?]</span>
          <span v-for="dot in number2" :key="dot" class="dot"></span>
        </template>
        <template v-else-if="level === 2">
          <p>{{ number1 }}</p>
          <span class="comparison">[?]</span>
          <p>{{ number2 }}</p>
        </template>
        <template v-else-if="level === 3">
          <p>{{ expression1 }}</p>
          <span class="comparison">[?]</span>
          <p>{{ expression2 }}</p>
        </template>
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
import {reactive, toRefs} from "vue";

export default {
  name: 'EduM',
  setup() {
    const state = reactive({
      level: 0,
      number1: 5,
      number2: 2,
      expression1: '',
      expression2: '',
      correctAnswers: 0,
      showResult: false,
      resultMessage: '',
      questionNumber: 0,
      answered: false
    });

    const selectLevel = (lvl) => {
      state.level = lvl;
      state.questionNumber = 0;
      nextQuestion();
    };

    const nextQuestion = () => {
      state.answered = false;
      if (state.questionNumber >= 10) {
        state.level = 0;
        state.questionNumber = 0;
        return;
      }
      state.questionNumber++;
      if (state.level === 1) {
        state.number1 = Math.floor(Math.random() * 10) + 1;
        state.number2 = Math.floor(Math.random() * 10) + 1;
      } else if (state.level === 2) {
        state.number1 = Math.floor(Math.random() * 100) + 1;
        state.number2 = Math.floor(Math.random() * 100) + 1;
      } else if (state.level === 3) {
        let number1 = Math.floor(Math.random() * 10) + 1;
        let number2 = Math.floor(Math.random() * 10) + 1;
        let number3 = Math.floor(Math.random() * 10) + 1;
        let number4 = Math.floor(Math.random() * 10) + 1;

        state.expression1 = `${number1} + ${number2}`;
        state.expression2 = `${number3} + ${number4}`;
        state.number1 = number1 + number2;
        state.number2 = number3 + number4;
      }
      state.showResult = false;
      state.resultMessage = '';
    };
    const checkAnswer = (selectedAnswer) => {
      if (state.answered) {
        return;
      }
      state.answered = true;
      state.showResult = true;
      let result1 = state.number1;
      let result2 = state.number2;
      console.log(result1);
      console.log("-------------")
      console.log(result2);
      let isCorrect = false;
      if (selectedAnswer === 'equal') {
        isCorrect = result1 === result2;
      } else if (selectedAnswer === true) {
        isCorrect = result1 > result2;
      } else if (selectedAnswer === false) {
        isCorrect = result1 < result2;
      }

      state.resultMessage = isCorrect ? 'Dobra odpowiedź!' : 'Zła odpowiedź.';
      if (isCorrect) state.correctAnswers++;
    };

    return {...toRefs(state), selectLevel, nextQuestion, checkAnswer};
  }
};
</script>

<style scoped>
.Page {
  background-image: url('@/assets/images/backgrounds/background-home.svg');
  background-size: cover;
  height: 98vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.game-area {
  width: 80%;
  height: 80%;
  background-color: rgba(255, 255, 255, 0.5);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border-radius: 15px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
  padding: 20px;
}

.question, .buttons, .result {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 20px;
}

.question {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  margin-bottom: 20px;
}

.dot {
  width: 20px;
  height: 20px;
  background-color: #3498db;
  border-radius: 50%;
  margin: 5px;
  display: inline-block;
  min-width: 20px;
}

.comparison, .result p {
  font-size: 24px;
  margin: 0 10px;
}

.buttons button {
  font-size: 16px;
  width: 80px;
  height: 40px;
  padding: 5px;
  flex-grow: 0;
  margin: 0 5px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.result button {
  font-size: 18px;
  padding: 15px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.score p {
  font-size: 18px;
  color: #27ae60;
  font-weight: bold;
  margin: 10px 0;
  background-color: #ebeeef;
  padding: 10px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
  text-align: center;
}

.level-button {
  font-size: 20px;
  padding: 15px 30px;
  margin: 10px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  background-color: #3294d5;
  color: white;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

</style>

