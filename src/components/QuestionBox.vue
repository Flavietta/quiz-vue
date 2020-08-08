<template>
  <div class="question-box-container">
    <b-jumbotron>
      <b-alert
        v-bind:show="submitted"
        v-bind:variant="correctAnswer() ? 'success' : 'danger'"
        ><p v-if="correctAnswer()">Correct Answer</p>
        <p v-else>Wrong Answer</p></b-alert
      >
      <template v-slot:lead>
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4" />

      <p>
        <b-list-group>
          <b-list-group-item
            @click.prevent="selectAnswer(answer)"
            v-for="(answer, index) in shuffledAnswers"
            :key="index"
            v-bind:active="!submitted && answer == selectedAnswer"
            v-bind:variant="
              submitted && answer == currentQuestion.correct_answer
                ? 'success'
                : submitted &&
                  answer == selectedAnswer &&
                  answer != currentQuestion.correct_answer
                ? 'danger'
                : ''
            "
          >
            {{ answer }}
          </b-list-group-item>
        </b-list-group>
      </p>

      <b-button
        v-show="!submitted"
        variant="primary"
        v-on:click="submitted = true"
        >Submit</b-button
      >
      <b-button @click="nextQuestion" v-show="submitted" variant="success"
        >Next</b-button
      >
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function,
  },
  data() {
    return {
      selectedAnswer: "",
      shuffledAnswers: [],
      submitted: false,
    };
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.submitted = false;
        this.selectedAnswer = "";
        this.shuffleAnswers();
      },
    },
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    },
    classObject(answer) {
      if (this.submitted && answer == this.currentQuestion.correct_answer) {
        return "success";
      } else {
        return "";
      }
    },
  },
  methods: {
    nextQuestion() {
      this.next();
      this.increment(this.correctAnswer());
    },

    correctAnswer() {
      if (this.selectedAnswer == this.currentQuestion.correct_answer)
        return true;
      else return false;
    },
    selectAnswer: function(answer) {
      //   alert(answer);
      if (!this.submitted) {
        this.selectedAnswer = answer;
      }
    },
    shuffleAnswers: function() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ];
      this.shuffledAnswers = _.shuffle(answers);
    },
  },
};
</script>

<style>
.btn {
  margin: 0px 5px;
}
.list-group {
  margin-bottom: 15px;
}
.list-group-item:hover {
  cursor: pointer;
  background-color: azure;
}
</style>
