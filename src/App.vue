<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <div class="card" id="find-card">
        <h1 class="is-size-3">Pokedex</h1>
        <input id="find-input" class="input is-rounded" type="text" placeholder="Buscar Pokemon" v-model="find">
        <button class="button is-fullwidth is-success is-outlined" @click="findMethod">Buscar</button>
      </div>
      <div v-for="(poke, index) in filteredPokemon" :key="poke.url">
      <Pokemon-item :name="poke.name" :url="poke.url" :num="index+1"/>
    </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import PokemonItem from './components/PokemonItem.vue'

export default {
  data(){
    return{
      pokemons: [],
      filteredPokemon: [],
      find: ''
    }
  },
  name: 'App',
  components: {
    PokemonItem
  },
  created: async function(){
    try{
      const pokemonsRes = await axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
      this.pokemons = pokemonsRes.data.results
      this.filteredPokemon = pokemonsRes.data.results
    }catch(err){
      console.log(err)
    }
  }, 
  methods: {
    findMethod(){
      if(this.find == "" || this.find == " "){
        this.filteredPokemon = this.pokemons
      }else{
        this.filteredPokemon = this.pokemons.filter(pokemon => pokemon.name == this.find)
        console.log(this.find, this.filteredPokemon)
      }
    }
  },
  computed: {
    /*
    findResult: function(){
      if(this.find == "" || this.find == " "){
        return this.pokemons
      }else{
        return this.pokemons.filter(pokemon => pokemon.name == this.find)
      }
    }
    */
  }
  
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#find-card{
  padding: 10px;
  margin-bottom: 5px;
}
#find-input{
  margin-bottom: 6px;
}
</style>
