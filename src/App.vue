<template>
  <ReservaJCPB />
</template>

<script>
import ReservaJCPB from './components/ReservaJCPB.vue'

export default {
  name: 'App',
  components: {
    ReservaJCPB
  }
}
</script>
<style>
html, body {
  background-color: #f9ecdb;
  touch-action: manipulation;
}

#app {
  font-family: 'Fira Sans', Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background-color: #f9ecdb;
  padding-left: 10px;
  padding-right: 10px;
  touch-action: manipulation;
}

.btn {
  font-family: 'Work Sans', 'Fira Sans', Avenir, Helvetica, Arial, sans-serif; 
}

.margin {
  color: transparent;
}

.tdn {
    text-decoration: none;
}
</style>