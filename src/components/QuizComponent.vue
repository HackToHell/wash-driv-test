<script setup>
import { reactive, ref } from 'vue'
import { quizData } from '../data.js'

const currentQuestionIndex = ref(0)
const currentQuestion = ref(quizData[currentQuestionIndex.value])
const answerSelected = ref(false)
const feedback = ref('')
const quizStarted = ref(false)
const totalQuestions = ref(quizData.length)
const progressBarStyle = reactive({
  width: '0%'
})
const isCorrect = ref(true)
const correctAnswers = ref(0)

//This function checks if the answer is correct and if it is, it updates a text box saying hey
// this is correct, if not it prints out the feedback.
function selectAnswer(choice) {
  if (currentQuestion.value.correct === choice) {
    feedback.value = 'Correct!'
    isCorrect.value = true
    correctAnswers.value++
  } else {
    feedback.value = 'Wrong! ' + currentQuestion.value.explanation
    isCorrect.value = false
  }
  answerSelected.value = true
}

function nextQuestion() {
  if (currentQuestionIndex.value < quizData.length) {
    currentQuestionIndex.value++
    currentQuestion.value = quizData[currentQuestionIndex.value]
    progressBarStyle.width = (currentQuestionIndex.value / totalQuestions.value) * 100 + '%'
    answerSelected.value = false
  } else {
    quizStarted.value = false
  }
}

function restartQuiz() {
  currentQuestionIndex.value = 0
  currentQuestion.value = quizData[currentQuestionIndex.value]
  answerSelected.value = false
  feedback.value = ''
  progressBarStyle.width = '0%'
  correctAnswers.value = 0
  quizStarted.value = true
}

</script>

<template>
  <div v-if="!quizStarted" class="flex items-center justify-center h-screen">
    <div class="text-center">
      <p class="text-2xl mb-4 text">Press button to start the quiz.</p>
      <button class="btn btn-primary" @click="quizStarted=!quizStarted">Start the quiz</button>
    </div>
  </div>
  <div v-if="quizStarted" class="quiz p-4">

    <div class="w-full bg-primary rounded-full dark:bg-accent mb-4">
      <div class="bg-primary text-xs font-medium text-accent-content text-center p-0.5 leading-none rounded-full"
           :style="progressBarStyle">
        {{ (currentQuestionIndex / totalQuestions) * 100 }}%
      </div>
    </div>

    <div v-if="currentQuestionIndex < quizData.length ">
      <div class="question text-3xl mb-4">{{ currentQuestion.question }}</div>
      <ul class="choices grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 w-full">
        <li v-for="(choice, index) in currentQuestion.choices" :key="index">
          <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded w-full h-full"
                  @click="selectAnswer(choice)">{{ choice }}
          </button>
        </li>
      </ul>
      <div v-if="answerSelected" class="mt-4">
        <p class="text-xl mb-2 text-neutral-content">{{ feedback }}</p>
        <button class="btn" :class="{'btn-success': isCorrect, 'btn-error': !isCorrect}" @click="nextQuestion">Next
          Question
        </button>
      </div>
    </div>
    <div v-else>
      <p class="text-2xl mb-4">Your total marks: {{ correctAnswers }}/{{ quizData.length }}</p>
      <button class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded" @click="restartQuiz">Restart
        Quiz
      </button>
    </div>
  </div>
</template>

