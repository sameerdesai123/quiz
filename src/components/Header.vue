<template>
<div>
  <b-navbar toggleable="sm" type="light" variant="light">
    <b-navbar-toggle target="nav-text-collapse"></b-navbar-toggle>

    <b-navbar-brand>Quiz App</b-navbar-brand>

    <b-collapse id="nav-text-collapse" is-nav>
      <b-navbar-nav>
        <b-nav-text>Score : {{correct}}/{{total}}</b-nav-text>
      </b-navbar-nav>
    </b-collapse>
  </b-navbar>
</div>
</template>

<script>
    export default {
        name: 'Header',
        props: {
            correct: Number,
            total: Number
        }
    }
</script>

<style scoped>
.header {
    color: blue;
}
</style>