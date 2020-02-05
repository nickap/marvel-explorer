<template>
  <div id="app">
    <h1>Marvel Explorer</h1>
    <MarvelExplorer></MarvelExplorer>
  </div>
</template>

<script>
import MarvelExplorer from "./components/MarvelExplorer.vue";

export default {
  name: "app",
  components: {
    MarvelExplorer
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
