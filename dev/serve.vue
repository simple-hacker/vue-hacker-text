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
    <vue-hacker-text />
  </div>
</template>
