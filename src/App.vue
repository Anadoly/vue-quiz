<template>
  <div id="app">
    <Header 
      :numOfCorrectAnswers="numOfCorrectAnswers"
      :numOfTotal="numOfTotal"
    />
    <b-container class="bv-example-row">
      <b-row>
        <b-col
          sm="6"
          offset="3"
        >
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from './components/Header.vue';
import QuestionBox from './components/QuestionBox.vue';

export default {
  name: 'App',
  components: {
    Header,
    QuestionBox,
  },
  data() {
    return {
      questions: [],
      index: 0,
      numOfCorrectAnswers: 0,
      numOfTotal: 0,
    }
  },
  mounted() {
    fetch('https://opentdb.com/api.php?amount=10&category=27&type=multiple', {
      method: 'get',
    })
    .then((response) => {
      return response.json();
    })
    .then((jsonData) => {
      this.questions = jsonData.results;
    })
  },
  methods: {
    next() {
      this.index++;
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.num0fCorrectAnswers++;
      }
      this.numOfTotal++;
    }
  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
