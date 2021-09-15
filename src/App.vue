<template>
  <Header />
  <router-view />
</template>

<script>
import Header from "@/components/Header";
export default {
  components: {
    Header,
  },
  setup() {},
};
</script>

<style>
#app {
  font-family: Verdana, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
body {
  margin: 0;
  padding: 0;
}
</style>
