<template>
    <div>
  <b-jumbotron >
      <h2>Your Score : {{ perc() }} %</h2>
    <b-button variant="primary" @click="reset">Restart</b-button>
  </b-jumbotron>
</div>
</template>

<script>
    export default {
        name: 'Result',
        props: {
            correct: Number,
            total: Number,
            reset: Function
        },
        methods: {
            perc() {
                return this.correct/this.total * 100;
            }
        },
    }
</script>

<style scoped>

</style>