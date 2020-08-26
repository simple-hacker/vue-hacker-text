<template>
  <div id="app">
    <h1>Hacker</h1>
    <div class="hacker-text">
      <vue-hacker-text text="Hacker Text" speed="slow"/>
    </div>
    <h1>Stars</h1>
    <div class="hacker-text">
      <vue-hacker-text text="Hacker Text" mode="stars" speed="slow"/>
    </div>
    <h1>Type</h1>
    <div class="hacker-text">
      <vue-hacker-text text="Hacker Text" mode="type" speed="slow"/>
    </div>
  </div>
</template>

<script>
import Vue from 'vue';
import VueHackerText from '@/vue-hacker-text.vue';

export default Vue.extend({
  name: 'ServeDev',
  components: {
    VueHackerText
  },
  methods: {
    start() {
      console.log('Decoding....')
    },
    finish() {
      console.log('FINISHED DECODING')
    }
  }
});
</script>

<style scoped>
  #app {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding-top: 30px;
  }

  h1 {
    color: #333333
  }

  .hacker-text {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
    font-size: 2em;
    font-weight: 600;
    letter-spacing: 0.1em;
    color: #151515;
    margin-top: 10px;
    margin-bottom: 10px;
  }
</style>