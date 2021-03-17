<template>
  <div id="app">
   
    <div class="column is-half is-offset-one-quarter"> 
      <header>
        <img src="./assets/pokedex.png" alt="Pokedex">
        <h1 class="is-size-1">Pokédex</h1>
      </header>
      <input class="input is-rounded" type="text" v-model="search" placeholder="Busque um pokemon da primeira geração">
      <button id="search-button" class="button is-fullwidth is-primary is-rounded" @click="searchPokemon">Buscar</button>
      <div v-for="(poke, i) in filteredPokemons" :key="poke.url">
        <Pokemon :name="poke.name" :url="poke.url" :num="i + 1" />
      </div>
    </div> 
  </div>
</template>

<script>
import axios from 'axios';
import Pokemon from './components/Pokemon';

export default {
  name: 'App',
  components: {
    Pokemon
  },
  data(){
    return {
      pokemons: [],
      filteredPokemons: [],
      search: '',
    }
  },
  created: function(){
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(response => {
      this.pokemons = response.data.results;
      this.filteredPokemons = response.data.results;
    })
  },
  methods: {
    searchPokemon: function(){
      this.filteredPokemons = this.pokemons;
      if(this.search == '' || this.search == ' '){
        this.filteredPokemons = this.pokemons;
       }else{
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name === this.search)
        console.log(this.filteredPokemons)
       }
    }
  },
  computed: {},

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

header img {
  width: 350px;
  height: auto;
}

#search-button {
  margin-top: 5px;
}
</style>
