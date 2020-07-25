<template>
  <div id="app">
    <app-header :numCorrect="numCorrect" :numTotalQues="numTotalQues"></app-header>
    <b-container class="bv-example-row" v-if="over">
      <b-row>
        <b-col>
          <app-over :numCorrectOver="numCorrect" :restart="restart"></app-over>
        </b-col>
      </b-row>
    </b-container>
    <b-container class="bv-example-row" v-else>
      <b-row>
        <b-col sm="8" offset="2">
          <app-quiz
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :nextBtn="next"
            :quizIncrementCorrectValue="incrementCorrectValue"
          ></app-quiz>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Quiz from "./components/Quiz.vue";
import Over from "./components/Over.vue";

export default {
  name: "App",
  components: {
    "app-header": Header,
    "app-quiz": Quiz,
    "app-over": Over,
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotalQues: 0,
      over: false,
    };
  },
  methods: {
    next() {
      if (this.index >= 0 && this.index < 4) {
        this.index++;
        this.numTotalQues++;
      } else {
        this.over = true;
        return;
      }
    },
    incrementCorrectValue(isCorrect) {
      //retreives the value of isCorrect from child quiz component
      if (isCorrect) {
        this.numCorrect++;
      }
      //finally sends back as props to another child header component
    },
    restart(isOver) {
      if (!isOver) {
        this.over = isOver;
        this.index=0;
        this.numCorrect=0;
        this.numTotalQues=0;
      }
    },
  },
  mounted() {
    const link =
      "https://opentdb.com/api.php?amount=5&category=31&type=multiple";
    fetch(link, {
      method: "get",
    })
      .then((response) => {
        return response.json();
      })
      .then((json) => {
        this.questions = json.results;
      });
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 3.75rem;
  font-family: "Courier New", Courier, monospace;
}
@media screen and (max-width: 600px) {
  .offset-2 {
    margin-left: 0 !important;
  }
}
</style>
