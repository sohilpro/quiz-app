<template>
  <main class="app">
    <div>
      <h1>Vue Quiz</h1>
    </div>
    <section class="quiz" v-if="!accomplishedQuestion">
      <div class="information">
        <span class="question">{{ getCurrentQuestion.question }}</span>
        <span class="score">Score {{ score }} / {{ questions.length }}</span>
      </div>
      <div class="option">
        <label
          v-for="(option, index) in getCurrentQuestion.options"
          :key="index"
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
        >
          <input
            type="radio"
            :name="getCurrentQuestion.index"
            :value="index"
            v-model="getCurrentQuestion.selected"
            :disabled="getCurrentQuestion.selected"
            @change="setQuestion"
          />
          <span>{{ option }}</span>
        </label>
        <button @click="nextQuestion" :disabled="!getCurrentQuestion.selected">
          {{
            getCurrentQuestion.index == questions.length - 1
              ? "Finish"
              : getCurrentQuestion.selected == null
              ? "Select An Option"
              : "Next Option"
          }}
        </button>
      </div>
      <h3>@Sohil_Dev</h3>
    </section>
    <section v-else>
      <h2>You have Finished The Quiz</h2>
      <p>Your Score: {{ score }} / {{ questions.length }}</p>
      <h3 style="opacity: .6; margin-top: 2rem;">@Sohil_Dev</h3>
    </section>
  </main>
</template>

<script setup>
import { ref, computed } from "vue";
const questions = ref([
  {
    question: "What is Vue.js?",
    answer: 0,
    options: [
      "A JavaScript framework ",
      "A Library",
      "A Vue state-managements",
    ],
    selected: null,
  },
  {
    question: "What is VueRouter?",
    answer: 1,
    options: ["A Vue framework", "A Vue Library", "A Vue state-managements"],
    selected: null,
  },
  {
    question: "What is Pinia and VueX",
    answer: 0,
    options: [
      "A Vue state-managements",
      "A Vue.js framework",
      "A meta Vue.js framework",
    ],
    selected: null,
  },
  {
    question: "What is Nuxt.js",
    answer: 2,
    options: [
      "A Vue.js framework",
      "A Vue state-managements",
      "A meta Vue.js framework",
    ],
    selected: null,
  },
]);
const accomplishedQuestion = ref(false);
const currentQuestion = ref(0);
const score = computed(() => {
  let val = 0;
  questions.value.map((q) => {
    if (q.selected == q.answer) {
      val++;
    }
  });
  return val;
});
const getCurrentQuestion = computed(() => {
  let question = questions.value[currentQuestion.value];
  question.index = currentQuestion.value;
  return question;
});
const setQuestion = (e) => {
  questions.value[currentQuestion.value].selected = e.target.value;
  e.target.value = null;
};
const nextQuestion = () => {
  if (currentQuestion.value < questions.value.length - 1) {
    currentQuestion.value++;
  } else {
    accomplishedQuestion.value = true;
  }
};
</script>
<style></style>