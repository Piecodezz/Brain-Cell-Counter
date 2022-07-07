<script setup>
import { ref, computed } from "vue";
const questions = ref([
  {
    question: "What is 1+1?",
    answer: 0,
    options: ["2", "100", "11"],
    selected: null,
  },
  {
    question: "Who invented the brain",
    answer: 2,
    options: ["Virat Kohli", "wanindu", "god"],
    selected: null,
  },
  {
    question: "What name given below is related to elephants",
    answer: 1,
    options: ["Gotabaya", "Pinnavalage", "Jagtah"],
    selected: null,
  },
  {
    question: "Who will win the LPL 2022",
    answer: 2,
    options: ["Kandy Falcons", "Oorawela podi lamai", "Jaffna Kings"],
    selected: null,
  },
  {
    question: "Try and Try and Trya nd Try and Try - Who said this thing",
    answer: 1,
    options: ["Wise old man123", "wanindu", "Socksrapatis"],
    selected: null,
  },
  {
    question: "Does wanindu need help",
    answer: 2,
    options: ["No", "Yes", "A lot"],
    selected: null,
  },
  {
    question: "Who is the president of China",
    answer: 1,
    options: ["Xing Jing Ping", "Winnie the pooh", "Ping mama"],
    selected: null,
  },
]);
const quizCompleted = ref(false);
const currentQuestion = ref(0);
const score = computed(() => {
  let value = 0;
  questions.value.map((q) => {
    if (q.selected != null && q.answer == q.selected) {
      console.log("correct");
      value += 82;
    }
  });
  return value;
});
const getCurrentQuestion = computed(() => {
  let question = questions.value[currentQuestion.value];
  question.index = currentQuestion.value;
  return question;
});
const SetAnswer = (e) => {
  questions.value[currentQuestion.value].selected = e.target.value;
  e.target.value = null;
};
const NextQuestion = () => {
  if (currentQuestion.value < questions.value.length - 1) {
    currentQuestion.value++;
    return;
  }

  quizCompleted.value = true;
};
</script>

<template>
  <main class="app">
    <h1>Brain Cell Counter TM</h1>

    <section class="quiz" v-if="!quizCompleted">
      <div class="quiz-info">
        <span class="question">{{ getCurrentQuestion.question }}</span>
        <span class="score">Score {{ score }}</span>
      </div>

      <div class="options">
        <label
          v-for="(option, index) in getCurrentQuestion.options"
          :for="'option' + index"
          :class="`option ${
            getCurrentQuestion.selected == index
              ? index == getCurrentQuestion.answer
                ? 'correct'
                : 'wrong'
              : ''
          } ${
            getCurrentQuestion.selected != null &&
            index != getCurrentQuestion.selected
              ? 'disabled'
              : ''
          }`"
          :key="index"
        >
          <input
            type="radio"
            :id="'option' + index"
            :name="getCurrentQuestion.index"
            :value="index"
            v-model="getCurrentQuestion.selected"
            :disabled="getCurrentQuestion.selected"
            @change="SetAnswer"
          />
          <span>{{ option }}</span>
        </label>
      </div>

      <button @click="NextQuestion" :disabled="!getCurrentQuestion.selected">
        {{
          getCurrentQuestion.index == questions.length - 1
            ? "Finish"
            : getCurrentQuestion.selected == null
            ? "Select an option"
            : "Next question"
        }}
      </button>
    </section>

    <section v-else>
      <h2 v-show="score > 50 && score < 200">
        You are missing a lot of brain cells
      </h2>
      <h2 v-show="score > 200 && score < 420">Eh, kinda good but needs more</h2>
      <h2 v-show="score > 420">Nice cells</h2>
      <p>Your score is {{ score }}</p>
    </section>
  </main>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Montserrat", sans-serif;
}
body {
  background-color: #271c36;
  color: #fff;
}
.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  height: 100vh;
}
h1 {
  font-size: 2rem;
  margin-bottom: 2rem;
}
.quiz {
  background-color: #382a4b;
  padding: 1rem;
  width: 100%;
  max-width: 640px;
}
.quiz-info {
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
}
.quiz-info .question {
  color: #8f8f8f;
  font-size: 1.25rem;
}
.quiz-info.score {
  color: #fff;
  font-size: 1.25rem;
}
.options {
  margin-bottom: 1rem;
}
.option {
  padding: 1rem;
  display: block;
  background-color: #271c36;
  margin-bottom: 0.5rem;
  border-radius: 0.5rem;
  cursor: pointer;
}
.option:hover {
  background-color: #2d213f;
}
.option.correct {
  background-color: #2cce7d;
}
.option.wrong {
  background-color: #ff5a5f;
}
.option:last-of-type {
  margin-bottom: 0;
}
.option.disabled {
  opacity: 0.5;
}
.option input {
  display: none;
}
button {
  appearance: none;
  outline: none;
  border: none;
  cursor: pointer;
  padding: 0.5rem 1rem;
  background-color: #2cce7d;
  color: #2d213f;
  font-weight: 700;
  text-transform: uppercase;
  font-size: 1.2rem;
  border-radius: 0.5rem;
}
button:disabled {
  opacity: 0.5;
}
h2 {
  font-size: 2rem;
  margin-bottom: 2rem;
  text-align: center;
}
p {
  color: #8f8f8f;
  font-size: 1.5rem;
  text-align: center;
}
</style>
