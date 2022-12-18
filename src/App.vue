<template>
  <MainPage />
</template>

<script>
import MainPage from './components/MainPage.vue'

export default {
  name: 'App',
  components: {
    MainPage
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  display: flex;
  justify-content: flex-end;
  background: #d4d2d2;
}
#app.dark {
  background: #1E1E1E;
}
body {
  overflow-x: hidden;
  background: #1E1E1E;
  margin: 0;
}
</style>
