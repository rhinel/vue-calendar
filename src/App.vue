<template>
  <div class="app">
    <vue-calendar highlight="#1D8CE0" :weekstart="1" selectdate="2017-5-10" :callback="callback"></vue-calendar>
  </div>
</template>

<script>
import vueCalendar from './vue-calendar.vue'

export default {
  name: 'app',
  methods: {
    callback (i) {
      console.log(i)
    }
  },
  components: {
    vueCalendar: vueCalendar
  }
}
</script>

<style lang="scss">
* {
  padding: 0;
  margin: 0;
}

body {
}

.app {
  max-width: 500px;
  background-color: #eee;
  margin: 0 auto;
}
</style>
