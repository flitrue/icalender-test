<template>
  <div id="app">
    <div style="260px">
      <i-calender :marks="marks" />
    </div>
  </div>
</template>

<script>
import iCalender from "icalender";

export default {
  name: 'app',
  data() {
    return {
      marks: [
        {
          start: "2019-09-10",
          end: "2019-09-10",
        },
        {
          start: "2019-09-13",
          end: "2019-09-15",
        }
      ]
    }
  },
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
