<template>
  <div id="app">
    <i-calender />
  </div>
</template>

<script>
import iCalender from "@/components/icalender";

export default {
  name: 'app',
  components: {
    iCalender
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
