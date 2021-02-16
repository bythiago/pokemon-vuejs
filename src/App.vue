<template>
  <div id="app">
    <notifications group="foo" position="top right"/>
    <Pokemon msg="Procure o Pokemon"/>
  </div>
</template>

<script>
import Pokemon from './components/Pokemon.vue'

import Vue from 'vue'
import VueTailwind from 'vue-tailwind'

// Import component
import Loading from 'vue-loading-overlay';
import Notifications from 'vue-notification'

// Import stylesheet
import 'vue-loading-overlay/dist/vue-loading.css';

// Init plugin
Vue.use(Loading);
Vue.use(Notifications);
Vue.use(VueTailwind)

export default {
  name: 'App',
  components: {
    Pokemon
  }
}
</script>