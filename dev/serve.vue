<template>
  <div id="app">
    <div class="cool-text">
      <vue-hacker-text text="Hacker Text" mode="stars" speed="slow" :probability="0.005" @decodingStarted="start" @decodingFinished="finish"/>
    </div>
    <div class="cool-text">
      <vue-hacker-text text="another one" speed="slow"/>
    </div>
    <div class="cool-text">
      <vue-hacker-text text="michael perks" mode="type" speed="fast"/>
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
  .cool-text {
    font-weight: 700;
    font-size: 3em;
    text-transform: uppercase;
  }
</style>