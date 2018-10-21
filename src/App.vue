<template>
  <section class="container">
    <div id="app">
      <h1 class="title">
        Langton's ant
      </h1>
      <ant />
    </div>
  </section>
</template>

<script>
import Ant from './components/Ant.vue'

export default {
  name: 'app',
  components: {
    Ant
  }
}
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

.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  color: #fff;
  text-shadow: 1px 1px 10px #000;
}
</style>
