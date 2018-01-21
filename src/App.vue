<template>
  <div id="app">
    <img src="./assets/logo.png">
    <h1 id="tableH1">Test Table</h1>
    <TableData></TableData>
  </div>
</template>

<script>
import TableData from './components/Table.vue'
export default {
  name: 'app',
  components: {
    TableData
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
#tableH1 {
  color: #cc33ff;
  font-family: 'Georgia', Helvetica, Arial, sans-serif;
}
</style>
