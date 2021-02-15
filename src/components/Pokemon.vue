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
        <div class="mt-5 sm:mt-5 sm:flex sm:justify-center">
          <button v-on:click="buscar(input)" class="uppercase p-3 flex items-center bg-blue-600 text-blue-50 max-w-max shadow-sm hover:shadow-lg rounded-full w-12 h-12 ">
            <svg width="32" height="32"  viewBox="0 0 32 32" style="transform: rotate(360deg);"><path d="M29 27.586l-7.552-7.552a11.018 11.018 0 1 0-1.414 1.414L27.586 29zM4 13a9 9 0 1 1 9 9a9.01 9.01 0 0 1-9-9z" fill="currentColor"></path></svg>
          </button>
          <button v-on:click="buscar()" class="uppercase p-3 flex items-center bg-yellow-500 text-blue-50 max-w-max shadow-sm hover:shadow-lg rounded-full w-12 h-12 ">
            <svg  width="32" height="32" preserveAspectRatio="xMidYMid meet" viewBox="0 0 32 32" style="transform: rotate(360deg);"><path d="M26 18A10 10 0 1 1 16 8h6.182l-3.584 3.585L20 13l6-6l-6-6l-1.402 1.414L22.185 6H16a12 12 0 1 0 12 12z" fill="currentColor"></path></svg>
          </button>
          <!-- <div class="rounded-md shadow">
            <a href="#" v-on:click="buscar(input)" class="w-full flex items-center justify-center px-8 py-3 border border-transparent text-base font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 md:py-4 md:text-lg md:px-10">
            Buscar
            </a>
          </div> -->
        </div>
      </div>
    </div>
    <!-- component -->
    
    <div v-if="$data.pokemon" class="px-10 w-1/2">
      <div class="flex flex-col items-center justify-center bg-white p-4 shadow rounded-lg h-full transform hover:scale-105 duration-300 ease-in-out">
				<!-- <div class="absolute left-0 top-0 h-16 w-16">6</div>
				<div class="absolute top-0 right-0 h-16 w-16 ...">7</div> -->
        <div class="inline-flex shadow-lg border border-gray-200 rounded-full overflow-hidden h-40 w-40">
					<img v-bind:src="$data.pokemon.image" class="h-full w-full">
				</div>
				<h2 lang="text-center" style="font-size: 3em; position: relative; top: -3%">{{ $data.pokemon.name }}</h2>
        <!-- <h4 class="mb-3 text-xl font-semibold tracking-tight text-gray-800">This is the title</h4> -->
        <!-- <span class="text-grey-darker"></span> -->
        <div class="pt-5">
          <div v-for="type in $data.pokemon.types" v-bind:key="type.type.name" class="mx-1 inline-flex items-center bg-white leading-none rounded-full p-2 shadow text-teal text-sm" v-bind:style="{ 'background-color': colorType(type.type.name) }">
            <span class="inline-flex text-white rounded-full h-6 px-3 justify-center items-center">
              {{type.type.name}}
            </span>
          </div>
        </div>
				<!-- <p class="text-xs text-gray-500 text-center mt-4">
					{{ $data.pokemon.description }}
				</p> -->
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
    colorType: function (type) {
      if(type == 'grass'){
        return '#10B981';
      }

      if(type == 'poison'){
        return '#BE185D';
      }

      if(type == 'fire'){
        return '#F59E0B';
      }

      if(type == 'normal'){
        return '#6B7280';
      }

      if(type == 'bug'){
        return '#10B981';
      }

      if(type == 'psychic'){
        return '#BE185D';
      }

      return '#000000'
    },
    buscar: function(input = null){
      try {
        var loader = this.loadShow()
        
        if(input === null){
          input = Math.floor(Math.random() * (811 - 0) + 0)
        } else {
          input.toLowerCase()
        }
        
        axios.get('https://pokeapi.co/api/v2/pokemon/' + input).then((response) => {
          //axios.get('https://pokeapi.co/api/v2/characteristic/' + response1.data.id).then(response => {
            this.loadHide(loader)
            this.pokemon = {
              id: response.data.id,
              name: response.data.name,
              image: `https://pokeres.bastionbot.org/images/pokemon/${response.data.id}.png`,
              types: response.data.types,
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