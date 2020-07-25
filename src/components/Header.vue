<template>
  <div class="header">
    <div>
      <b-nav tabs>
        <b-nav-item disabled>Quiz Contest</b-nav-item>
        <b-nav-item disabled>Questions: {{numTotalQues+1}}/5</b-nav-item>
        <b-nav-item disabled>Score: {{numCorrect}}/{{numTotalQues+1}}</b-nav-item>
      </b-nav>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    numCorrect: Number,
    numTotalQues: Number,
  },
};
</script>


<style scoped>
.nav-tabs .nav-item {
  font-size: 1.4rem;
}
.nav-tabs .nav-item:first-child {
  font-family: 'Times New Roman', Times, serif;
  font-weight: bold;
  font-size: 1.6rem;
}
.nav-tabs .nav-item:nth-child(2),.nav-tabs .nav-item:last-child{
  margin-left: auto;
}
@media screen and (max-width: 320px) {
.nav-tabs .nav-item:nth-child(2),.nav-tabs .nav-item:last-child{
  margin: auto;
}
.nav-tabs .nav-item {
  margin:auto;
  font-size: 1rem;
}

}
</style>
