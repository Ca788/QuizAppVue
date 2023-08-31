<script setup>
import { ref, computed } from 'vue'

const questions = ref([
  {
    question: 'Quem descubriou o Brasil ?',
    answer: 0,
    options: [
      'Pedro alvares cabral',
      'Talles de mileto',
      'Lula'

    ],
    selected: null
  },
  {
    question: ' Qual o pais mais populoso do mundo ?',
    answer: 0,
    options: [
      'China',
      'Estados unidos',
      'Japão'

    ],
    selected: null
  },
  {
    question: ' Quando aconteceu a segunda guerra mundial ?',
    answer: 2,
    options: [
      '1998',
      '1540',
      '1939'

    ],
    selected: null
  },
    {
    question: ' Quando aconteceu a segunda guerra mundial ?',
    answer: 2,
    options: [
      '1998',
      '1540',
      '1939'

    ],
    selected: null
  },
])

const quizCompleted = ref(false)
const currentQuestion = ref(0)

const score = computed(() => {
  let value = 0
  questions.value.map(q => {
    if (q.selected == q.answer) {
      value++
    }
  });
  return value;
});

const getCurrentQuestion = computed(() => {
  let question = questions.value[currentQuestion.value]
  question.index = currentQuestion.value
  return question
})

const SetAnswer = evt => {
  questions.value[currentQuestion.value].selected = evt.target.value;
  evt.target.value = null;
};

const NextQuestion = () => {
  if (currentQuestion.value < questions.value.length - 1) {
    currentQuestion.value++
  } else {
    quizCompleted.value = true
  }
};

</script>

<template>
  <main class="app">
    <h1>Quiz</h1>
    <section class="quiz" v-if="!quizCompleted">
      <div class="quiz-info">
        <span class="question">{{ getCurrentQuestion.question }}</span>
        <span class="score">Score {{ score }} / {{ questions.length }}</span>
      </div>

      <div class="options">
        <label v-for="(option, index) in getCurrentQuestion.options" 
        :key="index" 
        :class="`option 
        ${getCurrentQuestion.selected == index
            ? index == getCurrentQuestion.answer
              ? 'correct'
              : 'wrong'
            : ''
          } ${getCurrentQuestion.selected != null &&
            index != getCurrentQuestion.selected
            ? 'disabled'
            : ''
          }`">
          <input type="radio" :name="getCurrentQuestion.index" :value="index" v-model="getCurrentQuestion.selected"
            :disabled="getCurrentQuestion.selected" @change="SetAnswer">
          <span>{{ option }}</span>
        </label>
      </div>

      <button 
      @click="NextQuestion" 
      :disabled="!getCurrentQuestion.selected">
        {{

          getCurrentQuestion.index == questions.length - 1
          ? 'finish'
          : getCurrentQuestion.selected == null
            ? ' selected an option'
            : ' next question'
        }}
      </button>
    </section>

    <section v-else>
      <h2>Você finalizou o quiz</h2>
      <p>Seu score foi {{ score }} / {{ questions.length }}</p>
    </section>
  </main>
</template>



<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "MontSerrat", sans-serif;
}

body {
  background-color: #36254d;
  color: #fff;
}

.app {
  display: flex;
  flex-direction: column;
  text-align: center;
  padding: 2rem;
  min-height: 100vh;
}

h1 {
  font-size: 2em;
  margin-bottom: 2rem;
}

.quiz {
  background-color: #4e376e;
  padding: 1rem;
  width: 100%;
  max-width: 640px;
  border-radius: 0.5rem;
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

.quiz-info .score {
  color: #fff;
  font-size: 1.25rem;
}

.options {
  margin-bottom: 1rem;
}

.option {
  display: block;
  padding: 1rem;
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
  color: #fff;
  font-weight: 700;
  text-transform: uppercase;
  font-size: 1.25rem;
  border-radius: 0.5rem;
}

button.disabled {
  opacity: 0.5;
}

h2 {
  font-size: 2rem;
  margin-bottom: 2rem;
  text-align: center;
}

p {
  color: #8f8f8f;
  font-size: 1.25rem;
  text-align: center;
}
</style>
