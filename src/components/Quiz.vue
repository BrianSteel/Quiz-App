<template>
  <div id="quiz">
    <div>
      <b-jumbotron>
        <h1>Question And Answer</h1>
        <p>{{parseQues()}}</p>
        <hr class="my-4" />
        <b-list-group>
          <b-list-group-item
            v-for="(answer, index) in shuffled_array"
            :key="answer"
            @click="selectAns(index)"
            :class="chooseClass(index)"
          >{{parseAns(answer)}}</b-list-group-item>
        </b-list-group>
        <b-button
          variant="primary"
          @click.prevent="submit"
          :disabled="selectedAns===null || 
        questionAnswred===true"
        >Submit</b-button>
        <b-button variant="success" v-if="questionAnswred" @click="nextBtn">Next</b-button>
      </b-jumbotron>
    </div>
  </div>
</template>

<script>
import _ from "lodash";

export default {
  props: {
    currentQuestion: Object,
    nextBtn: Function,
    quizIncrementCorrectValue: Function,
  },
  data() {
    return {
      selectedAns: null,
      shuffled_array: [],
      correctIndex: null,
      questionAnswred: false,
    };
  },
  methods: {
    parseQues() {
      const parsedString = this.currentQuestion.question
        .replace(/&quot;/g, '"')
        .replace(/&#039;/g, "'");
      return parsedString;
    },
    parseAns(answer) {
      answer = answer.replace(/&quot;/g, '"').replace(/&#039;/g, "'");
      return answer;
    },
    selectAns(index) {
      this.selectedAns = index;
    },
    shuffle() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ];
      this.shuffled_array = _.shuffle(answers);
      this.correctIndex = this.shuffled_array.indexOf(
        this.currentQuestion.correct_answer
      );
    },
    submit() {
      let isCorrect = false;
      if (
        this.selectedAns ===
        this.correctIndex
      ) {
        isCorrect = true;
      }
      this.quizIncrementCorrectValue(isCorrect); //connects through props, sends data to root component and returns
      this.questionAnswred = true;
    },
    chooseClass(index) {
      if (!this.questionAnswred && this.selectedAns === index) {
        return "selected";
      } else if (this.questionAnswred && index === this.correctIndex) {
        return "correct";
      } else if (this.selectedAns === index && index !== this.correctIndex) {
        return "incorrect";
      }
    },
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedAns = null; 
        this.questionAnswred = false;
        this.shuffle();
      },
    },
  },
  computed: {
    answers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ];
      return answers;
    },
  },
};
</script>


<style scoped>
#quiz {
  word-wrap: break-word;
}
h1 {
  color: #0f4c75;
  font-weight: bold;
  font-size: 2rem;
  padding: 1rem;
}
p {
  color: #0f4c75;
  height: 4rem;
}
.list-group {
  margin-bottom: 1rem;
}

button {
  margin: 1rem;
}
.list-group-item:hover {
  background: #eee;
  cursor: pointer;
}
.selected {
  background: lightblue;
}
.selected:hover {
  background: lightblue;
}
.correct {
  background: limegreen;
}
.correct:hover {
  background: rgb(57, 233, 57);
}
.incorrect {
  background: maroon;
  color: white;
}
.incorrect:hover {
  background: rgb(173, 1, 1);
  color: white;
}
@media screen and (max-width: 320px) {
  #quiz {
    font-size: 0.8rem!important;
  }
  h1{
    font-size: 1.2rem!important;
  }
}
</style>
