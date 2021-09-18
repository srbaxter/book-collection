<template>
  <Header />
  <main>
    <router-view />
  </main>
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
  font-family: Verdana, Tahoma, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
body {
  background-color: rgba(254, 255, 253, 0.945);
}
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>
