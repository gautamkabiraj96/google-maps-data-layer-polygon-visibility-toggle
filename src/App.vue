<template>
  <div id="app">
    <PolygonDataLayer />
  </div>
</template>

<script>
import PolygonDataLayer from "./components/PolygonDataLayer";

export default {
  name: "App",
  components: {
    PolygonDataLayer,
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
