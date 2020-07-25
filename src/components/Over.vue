<template>
  <div id="over">
    <b-jumbotron>
      <p>
        You got {{numCorrectOver}}
        <span v-if="numCorrectOver===1">answer</span>
        <span v-else>answers</span> correct out of 5 questions
      </p>
      <b-button variant="primary" href="#" @click="reask">Again</b-button>
      <b-button variant="success" href="#" @click="reload">More Questions</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
export default {
  props: {
    numCorrectOver: Number,
    restart: Function,
  },
  methods: {
    reask() {
      let isOver = false;
      this.restart(isOver);
    },
    reload() {
      location.reload();
    },
  },
};
</script>

<style scoped>
p {
  padding: 2rem;
  font-size: 3rem;
  font-family: "Times New Roman", Times, serif;
  color: black;
}
.btn-success {
  margin-left: 1rem;
}
@media screen and (max-width: 600px) {
  p {
    font-size: 1.8rem;
  }
}

@media screen and (max-width: 320px) {
  p {
    font-size: 1.2rem;
  }
}
</style>