<template>
    <b-jumbotron>
      <template slot="lead">
        {{ currentQuestion.question }}
      </template>


      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in shuffledAnswers"
          :key="index"
          @click.prevent="selectAnswer(index)"
          :class="answerClass(index)"
        >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>


      <b-button
        variant="primary"
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered"
      >
        Submit
      </b-button>
      <b-button @click="next">Next</b-button>
    </b-jumbotron>
</template>
<script>
import _ from 'lodash'

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index
    },
    shuffleAnswers() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
      this.shuffledAnswers = _.shuffle(answers)
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)

    },
    answerClass(index) {
      let answerClass = ''

      if (this.answered) {
        if (index === this.correctIndex) {
          answerClass = 'correct'
        }  else if (index === this.selectedIndex) {
          answerClass = 'incorrect'
        }
      } else if (index === this.selectedIndex) {
        answerClass = 'selected'
      }

      return answerClass
    },
    submitAnswer() {
      let isCorrect = false

      if(this.selectedIndex === this.correctIndex) {
        isCorrect = true
      }

      this.answered = true

      this.increment(isCorrect)
    }
  },
  computed: {

  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null
        this.answered = false
        this.shuffleAnswers()
      }
    }
  }
}
</script>
<style scoped>
  .list-group {
    margin-bottom: 15px;
  }

  .list-group-item:hover {
    background: #EEE;
    cursor: pointer;
  }

  .selected {
    background-color: lightblue;
  }

  .correct {
    background-color: lightgreen;
  }

  .incorrect {
    background-color: red;
  }
  .btn {
    margin: 0 5px;
  }

</style>


