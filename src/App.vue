<template>
  <section class="container">
    <div id="app">
      <button @click="ant=!ant">Change BG</button>
      <div v-if="ant">
        <h1 class="title black">
          Langton's ant 
        </h1>
        <ant />
      </div>
      <div v-else>
        <h1 class="title white">
          Grid Wave
        </h1>
        <wave />
      </div>
    </div>
  </section>
</template>

<script>
import Ant from './components/Ant.vue'
import Wave from './components/Wave.vue'

export default {
  name: 'app',
  components: {
    Ant,
    Wave
  },
  data() {
    return {
      ant: true
    }
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
  text-align: center;
}

.title {
  font-size: 3em;
}

.black {
  color: black;
  text-shadow: 1px 1px 10px gray;
}

.white {
  color: white;
  text-shadow: 1px 1px 10px white;
}
</style>
