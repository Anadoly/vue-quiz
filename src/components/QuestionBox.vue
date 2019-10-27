<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>
        {{ currentQuestion.question }}
      </template>
      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in answers"
          :key="index"
          :class="answerClass(index)"
          @click.prevent="selectAnswer(index)"
        >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>
      

      <b-button
        variant="primary"
        :disabled="selectedIndex === null || answered"
        @click="submitAnswer"
      >
        Submit
      </b-button>
      <b-button
        variant="success"
        @click="next"
      >
        Next
      </b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'loadsh';
export default {
  props: {
    currentQuestion: {
      type: Object,
      required: true,
    },
    next: {
      type: Function,
      required: true,
    },
    increment: {
      type: Function,
      required: true,
    }
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: null,
      answered: false,
    }
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
      return answers;
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.answered = false;
        this.shuffleAnswers()
      }
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index; 
    },
    shuffleAnswers() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    },
    submitAnswer() {
      let isCorrect =false;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.answered = true;
      this.increment(isCorrect); 
    },
    answerClass(index) {
      let answerClass = '';
      if (!this.answered && this.selectedIndex === index) {
        answerClass =  'selected';
      } else if (this.answered && this.correctIndex === index) {
        answerClass =  'correct';
      } else if (this.answers && this.selectedIndex === index && this.correctIndex !== index) {
        answerClass =  'incorrect';
      }
      return answerClass;
    }
  },
}
</script>

<style scoped>
  .question-box-container {
    margin: 20px auto;
  }

  .list-group {
    margin: 15px;
  }

  .list-group-item:hover {
    background: #eee;
    cursor: pointer;
  }

  .btn {
    margin: 0 5px;
  }

  .selected {
    background: lightblue;
  }

  .correct {
    background: lightgreen;
  }

  .incorrect {
    background: red;
  }
</style>
