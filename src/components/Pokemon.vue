<template>
  <div class="flex justify-center mt-20">
    <notifications group="foo" position="top right"/>
    <!-- <div class="mt-10 mx-auto max-w-7xl px-4 sm:mt-12 sm:px-6 md:mt-16 lg:mt-20 lg:px-8 xl:mt-28"> -->
    <div class="px-10 w-1/2">
      <div class="sm:text-center lg:text-left px-5 py-5 bg-white shadow rounded-lg h-full">
        <h1 class="text-4xl tracking-tight font-extrabold text-gray-900 sm:text-5xl md:text-6x l text-center">
          <h2 class="block xl:inline text-center">{{ msg }}</h2>
        </h1>
        <form action="#">
          <p class="text-center"></p>
          <div class="max-w-sm mx-auto p-1 pr-0 flex items-center">
            <autocomplete
              v-if="options"
              v-model="input"
              :source="options"
              class="flex-1 appearance-none rounded shadow p-3 text-grey-dark mr-2 focus:outline-none"
            >
            </autocomplete>
          </div>
        </form>
        <div class="mt-5 sm:mt-8 sm:flex sm:justify-center">
          <div class="rounded-md shadow">
            <a href="#" v-on:click="buscar(input)" class="w-full flex items-center justify-center px-8 py-3 border border-transparent text-base font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 md:py-4 md:text-lg md:px-10">
            Buscar
            </a>
          </div>
        </div>
      </div>
    </div>
    <!-- component -->
    
    <div v-if="$data.pokemon" class="px-10 w-1/2">
      <div class="flex flex-col items-center justify-center bg-white p-4 shadow rounded-lg h-full">
				<div class="inline-flex shadow-lg border border-gray-200 rounded-full overflow-hidden h-40 w-40">
					<img v-bind:src="$data.pokemon.image" class="h-full w-full">
				</div>
				<h2 lang="text-center mb-10" style="font-size: 3em;">{{ $data.pokemon.name }}</h2>
				<p class="text-xs text-gray-500 text-center mt-4">
					{{ $data.pokemon.description }}
				</p>
			</div>
    </div>
  </div>
</template>

<script>

const axios = require('axios')

import Vue from 'vue';

// Import component
import Loading from 'vue-loading-overlay';
import Notifications from 'vue-notification'
import Autocomplete from 'vuejs-auto-complete'

// Import stylesheet
import 'vue-loading-overlay/dist/vue-loading.css';

// Init plugin
Vue.use(Loading);
Vue.use(Notifications);

export default {
  name: 'Pokemon',
  data() {
    return {
      input: null,
      pokemon: null,
      options: null
    }
  },
  components: {
    Autocomplete
  },
  props: {
    msg: String
  },
  methods: {
    buscaPokemons: function(){
      axios.get('https://pokeapi.co/api/v2/pokemon/?limit=811').then((response) => {
        this.options = response.data.results.map(item => {
          return {
            id: item.name,
            name: item.name
          } 
        })
      });
    },
    buscar: function(input){
      try {
        var loader = this.loadShow()
        axios.get('https://pokeapi.co/api/v2/pokemon/' + input.toLowerCase()).then((response) => {
          //axios.get('https://pokeapi.co/api/v2/characteristic/' + response1.data.id).then(response => {
            this.loadHide(loader)
            this.pokemon = {
              name: response.data.name,
              image: `https://pokeres.bastionbot.org/images/pokemon/${response.data.id}.png`,
              description: response//response.data.descriptions[2].description
            }
          //})
          // .catch((error) => {
          //   this.showNotify(error)
          //   this.loadHide(loader)
          // });
        })
        .catch((error) => {
          this.showNotify(error)
          this.loadHide(loader)
        });
      } catch (error) {
        this.showNotify(error)
        this.loadHide(loader)
      }
    },
    loadShow(){
      return this.$loading.show({
        // Optional parameters
        container: this.fullPage ? null : this.$refs.formContainer,
        canCancel: true,
        onCancel: this.onCancel,
      });
    },
    loadHide(loader){
      setTimeout(() => {
        loader.hide()
      }, 2000)
    },
    showNotify(error){
      this.pokemon = null
      this.$notify({
        group: 'foo',
        title: 'Important message',
        text: error,
        type: 'error'
      });
    }
  },
  beforeMount() {
    console.log('//beforeMount')
  },
  mounted() {
    this.buscaPokemons()
    console.log('//mounted')
  }
}
</script>