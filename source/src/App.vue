<template>
  
  <div id="app">
    <h1 id="title">Pokedex</h1>
    <input type="text" placeholder="Buscar pokemon pelo nome" v-model="busca"  class="input is-rounded">
    <button class="button" id="buscaBtn" @click="buscar">Pesquisar</button>
  <div id="pokedex">
    <div v-for="(poke,index) in filteredPokemons" :key="poke.url">
      <Pokemon :name="poke.name" :url="poke.url" :num="index+1"/>
    </div>
  </div>
  </div>
</template>

<script>
import axios from 'axios';
import Pokemon from './components/Pokemon';
export default {
  name: 'App',
  data (){
    return{
      pokemons: [],
      filteredPokemons: [],
      busca: ''
    }
  },
  created: function(){
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=200&offset=100").then(res =>
    {
      console.log("Pegou a lista de pokemons");
      this.pokemons = res.data.results;
      this.filteredPokemons = res.data.results;
    })
  },
  components:{
    Pokemon
  },

  methods:{
    buscar: function(){
      this.filteredPokemons = this.pokemons;
      if(this.busca == '' || this.busca == ' '){
        this.filteredPokemons = this.pokemons;
      }
      else{
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name == this.busca);
      }
    }
    },
}
</script>

<style>
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
	background: linear-gradient(to right, rgb(240, 120, 202), rgb(131, 232, 252));
	color: white;
	margin: 0;
	
}
#title{
  font-size: 50px;
  text-shadow: 1px 1px black;
  animation-duration: 10s;
  animation-direction: alternate;
  animation-name: example;
  animation-iteration-count: infinite;
}
#pokedex{

  display: grid;
	grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
	grid-gap: 20px;
	padding-inline-start: 0;
  padding: 2%;
}

#buscaBtn{
  background: #fc3f4f;
  width: 200px;
  margin: 1% 0% 1% 0%;
}

@keyframes example {
  0%   {color:red;}
  25%  {color:yellow;}
  50%  {color:blue;}
  75%  {color:green;}
  100% {color:purple;}
}

</style>
