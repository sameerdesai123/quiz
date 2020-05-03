<template>
  <div id="app">
      <Header :correct="total_correct" :total="total_answered" />
      <hr class="my-4">
      <b-container class="bv-example-row">
        <b-row>
          <b-col sm=6 offset=3>
            <QuestionBox 
            v-if="questions.length && playing"
            :currentQuestion="questions[index]" 
            :nextQuestion="increments" 
            :increment="increment"
            />
            <Result 
            v-if="playing === false"
            :correct="total_correct"
            :total="total_answered"
            :reset="reset"
            />
          </b-col>
        </b-row>
      </b-container>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'
import Result from './components/Result.vue'

export default {
  name: 'App',
  components: {
    Header,
    QuestionBox,
    Result
  },
  data() {
    return {
      questions: [],
      index: 0,
      total_correct: 0,
      total_answered: 0,
      playing: true
    }
  },
  methods: {
    increments() {
      return this.index += 1;
    },
    increment(isCorrect) {
      if(isCorrect){
        this.total_correct += 1;
      }
      this.total_answered += 1;
      if (this.total_answered == 10){
        this.playing = false;
      }
    },
    reset() {
      this.playing = true;
      this.getQuestions()
      this.total_correct = 0;
      this.total_answered = 0 
      this.index = 0
    },
    getQuestions() {
      fetch('https://opentdb.com/api.php?amount=10&category=27&type=multiple', {
      method: 'get'
    })
    .then((res) => {
      return res.json();
    })
    .then( (jsondata) => {
      this.questions = jsondata.results;
    })
    }
  },
  mounted () {
    this.getQuestions();
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
