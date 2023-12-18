<template>
  <div id="app">
    <ArenaBall msg="Welcome to Your Vue.js App" />
  </div>
</template>

<script>
import ArenaBall from "./components/ArenaBall.vue";

export default {
  name: "App",
  components: {
    ArenaBall,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  height: 100vh;
}
</style>
