<template>
  <div id="app">
    <SaveCalculator msg="Welcome to Your Vue.js App" />
  </div>
</template>

<script>
import SaveCalculator from "./components/SaveCalculator.vue";

export default {
  name: "app",
  components: {
    SaveCalculator
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
  font-size: 14px;
}
</style>
