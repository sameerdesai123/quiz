<template>
    <b-jumbotron style="font-size=10px">
    <template v-slot:lead>
      {{ currentQuestion.question}}
    </template>
      <b-list-group >
        <b-list-group-item 
          @click="selectAnswer(index)"
          v-for="(option, index) in shuffledAnswers" :key="index"
          :class="[
          !answered && selectedIndex === index ? 'selected':
          answered && index === correctIndex ? 'correct':
          answered && index === selectedIndex ? 'incorrect' : '']"
        >
          {{option}}
        </b-list-group-item>
      </b-list-group>
    <b-button variant="primary" @click="submitAnswer"
      :disabled="selectedIndex === null || answered === true"
    >
      Submit
    </b-button>
    <b-button variant="success" @click="nextQuestion">Next</b-button>
  </b-jumbotron>
</template>

<script>
import _ from 'lodash';

    export default {
        name: 'QuestionBox',
        props: {
            currentQuestion: Object,
            nextQuestion: Function,
            increment: Function
        },
        data() {
          return {
            selectedIndex: null,
            shuffledAnswers: [],
            answered: false,
            correctIndex: null,
          }
        },
        watch: {
          currentQuestion: {
            immediate: true,
            handler() {
              this.selectedIndex = null;
              this.shuffleAnswers();
              this.answered = false;
            }
          } 
          // {
          //   this.selectedIndex = null;
          //   this.shuffleAnswers();
          // }
        },
        computed: {
          answers() {
            let options = [...this.currentQuestion.incorrect_answers];
            options.push(this.currentQuestion.correct_answer);
            return options
          }
        },
        methods: {
          selectAnswer(index) {
            this.selectedIndex = index;
          },
          shuffleAnswers() {
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.shuffledAnswers = _.shuffle(answers)
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);
          },
          submitAnswer() {
            let isCorrect = this.shuffledAnswers[this.selectedIndex] === this.currentQuestion.correct_answer;
            this.answered = true;
            return this.increment(isCorrect);
          }
        },
        // mounted () {
        //   this.shuffleAnswers();
        // },
    }
</script>

<style scoped>

.list-group {
  margin: 15px 0px;
}

.list-group-item:hover {
  margin: 2px 0px;
  background-color: rgba(255, 255, 255 , 0.5);
  cursor: pointer;
}

.btn {
  margin: 0px 10px;
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
</style>
