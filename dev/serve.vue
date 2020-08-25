<script>
import Vue from 'vue';
import VueHackerText from '@/vue-hacker-text.vue';

export default Vue.extend({
  name: 'ServeDev',
  components: {
    VueHackerText
  }
});
</script>

<template>
  <div id="app">
    <vue-hacker-text text="Sample text" speed="slow" :probability="0.5"/>
    <vue-hacker-text text="another one" speed="medium"/>
    <vue-hacker-text text="michael perks" speed="fast" :probability="0.01"/>
  </div>
</template>
