<template>
  <div id="app">
    <vue-daterange-picker double start-date="06/10/2017" end-date="06/10/2018"/>
  </div>
</template>

<script>
import VueDaterangePicker from './components/vue-daterange-picker';

export default {
  name: 'app',
  components: {
    VueDaterangePicker
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
