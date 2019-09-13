<template>
  <div id="app">
    <div style="width: 260px">
      <i-calender :marks="marks" @forward="change" @back="change" />
    </div>
  </div>
</template>

<script>
import iCalender from "icalender";

export default {
  name: "app",
  data() {
    return {
      marks: [
        {
          start: "2019-09-10",
          end: "2019-09-10"
        },
        {
          start: "2019-09-13",
          end: "2019-09-15"
        },
        {
          start: "2019-09-28",
          end: "2019-10-03"
        }
      ]
    };
  },
  components: {
    iCalender
  },
  methods: {
    change(date) {
      console.log(date); // eslint-disable-line
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
