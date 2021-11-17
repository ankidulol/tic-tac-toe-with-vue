<template>
  <Header msg="Tic Tac Toe With Vue" />
  <Board />
</template>

<script>
import Header from "./components/Header.vue";
import Board from "./components/Board.vue";

export default {
  name: "App",
  components: {
    Header,
    Board,
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Permanent%20Marker");
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
h1,
h2 {
  font-family: Permanent Marker;
}
</style>
