<template>
  <div class="w-full">
    <div class="flex px-10 mt-10 grid grid-cols-1 md:grid-cols-2">
      <div class="w-full">
        <div class="bg-white shadow-lg rounded-lg px-4 py-6 mx-4">
          <center>
            <img class="pt-5 object-cover md:w-24" src="../assets/logo.png" width="100">
          </center>
          <h2 class="font-bold text-5xl mt-4">{{ msg }}</h2>
          <autocomplete
            v-if="options"
            v-model="input"
            :source="options"
            class="px-10 py-5 rounded-full">
          </autocomplete>
          <div class="text-center">
            <button
              v-on:click="buscar(input)"
              type="button"
              class="border border-indigo-500 bg-indigo-500 text-white rounded-md px-4 py-2 m-2 transition duration-500 ease select-none hover:bg-indigo-600 focus:outline-none focus:shadow-outline"
            >
              Buscar
            </button>
            <button
              v-on:click="buscar()"
              type="button"
              class="border border-green-500 bg-green-500 text-white rounded-md px-4 py-2 m-2 transition duration-500 ease select-none hover:bg-green-600 focus:outline-none focus:shadow-outline"
            >
              Aleatório
            </button>
          </div>
        </div>
      </div>
      <div v-if="$data.pokemon" class="w-full sm:mt-0 mt-10">
        <div class="col-span-6 sm:col-span-6 md:col-span-3 lg:col-span-2 xl:col-span-2 h-full">
          <div class="bg-white shadow-lg rounded-lg px-4 py-6 mx-4 h-full">
            <div class="mx-auto h-40 bg-gray-200 rounded-md ">
              <center>
                <img v-bind:src="$data.pokemon.image" class="h-48 w-full object-cover md:w-48">
              </center>
            </div>
            <h2 class="font-bold text-4xl mt-4 pt-5">{{ $data.pokemon.name }}</h2>
            <div class="h-2 bg-gray-200 w-64 mt-2 block mx-auto rounded-sm"></div>
            <div class="flex justify-center mt-4">
              <div 
                v-for="type in $data.pokemon.types" 
                v-bind:key="type.type.name" v-bind:style="{ 'background-color': colorType(type.type.name) }" 
                class="rounded-sm text-white h-6 px-3 mx-1 justify-center items-center">{{type.type.name}}
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>

const axios = require('axios')
import Autocomplete from 'vuejs-auto-complete'

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
            this.loadHide(loader)
            this.pokemon = {
              id: response.data.id,
              name: response.data.name,
              image: `https://pokeres.bastionbot.org/images/pokemon/${response.data.id}.png`,
              types: response.data.types,
              description: response//response.data.descriptions[2].description
            }
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
    //console.log('//beforeMount')
  },
  mounted() {
    this.buscaPokemons()
    //console.log('//mounted')
  }
}
</script>