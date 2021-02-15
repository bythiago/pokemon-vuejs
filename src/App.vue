<template>
  <div id="app">
    <link href="//db.onlinewebfonts.com/c/f4d1593471d222ddebd973210265762a?family=Pokemon" rel="stylesheet" type="text/css"/>
    <Pokemon msg="Procure o Pokemon"/>
  </div>
</template>

<script>
import Pokemon from './components/Pokemon.vue'

export default {
  name: 'App',
  components: {
    Pokemon
  }
}
</script>

<style>
  html {
    background-image: url('https://i.pinimg.com/originals/ca/fe/c2/cafec2809887a1c37c83d9ebbc953252.jpg');
    background-size: cover;
    background-repeat: no;
  }
  h2 {
    font-family: 'Pokemon';
    -webkit-text-fill-color: #ffcb05; /* Will override color (regardless of order) */
    -webkit-text-stroke-width: 2.5px;
    -webkit-text-stroke-color: #2a75bb;
    letter-spacing: 1vh;
  }
/*   
  div * {
    border: 1px solid red;
  } */

  html {
    overflow: hidden;
  }
</style>