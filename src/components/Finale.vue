<template>
  <div class="question-box-container">
    <b-jumbotron>
      <b-card>
        <b-card-body
          >Congratulations: {{ numCorrect }}/{{ numTotal }}</b-card-body
        >
      </b-card>
      <b-button @click="restartGame" variant="success">
        Restart Game
      </b-button>
    </b-jumbotron>
  </div>
</template>


<script>
export default {
  props: {
    restartGame: Function,
    numCorrect: Number,
    numTotal: Number,
  },
};
</script>

<style>
</style>
