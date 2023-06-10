<script>
import axios from 'axios';
export default {
  data() {
    return {
      searchInput: '',
      pokemon: null,
      jaName: '',
      allPokemons: null,
      nextPokemonX: 0,
      currentPokemonX: 0
    };
  },
  methods: {
    loadPokemons() {
      axios.get('https://pokeapi.co/api/v2/pokemon')
        .then(response => {
          this.allPokemons = response.data;
          this.searchInput = '';
        })
        .catch(error => {
          console.log(error);
        })
    },
    searchPokemon() {
      axios.get(`https://pokeapi.co/api/v2/pokemon/${this.searchInput.toLowerCase()}`)
        .then(response => {
          this.pokemon = response.data;
          this.searchInput = '';
          this.getJaName();
        })
        .catch(error => {
          console.log(error);
        })
    },
    getJaName() {
      if (this.pokemon) {
        axios.get(this.pokemon.species.url)
        .then(response => {
          this.jaName = response.data.names[9].name;          
        })
        .catch(error => {
          console.log(error);
        })
      }
    }
  }
}
</script>

<template>
  <div class="nav">
    <a href="https://fontmeme.com/pokemon-font/"><img src="https://fontmeme.com/permalink/230310/30c743499ebda3e82d6d7431347e5d1d.png" alt="pokemon-font"></a>
    <div class="search">
      <input v-model="searchInput" type="text" placeholder="Ingrese el nombre o ID del Pokémon"
        @keyup.enter="searchPokemon">
      <button @click="searchPokemon">Buscar</button>  
      <button @click="loadPokemons">Todos</button>  
    </div>
  </div>
  <div class="pokeData" v-if="pokemon">
    <div class="info">
      <h2>{{ pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1) }}</h2>
      <hr>
      <h4>Altura: {{ pokemon.height / 10}} m</h4>
      <hr>
      <h4>Peso: {{ pokemon.weight / 10 }} kg</h4>
      <hr>
      <h4>Habilidades:</h4>
      <h5 v-for="habilidad in pokemon.abilities">{{ habilidad.ability.name }}</h5>
    </div>
    <div class="pokemon">
      <img :src="pokemon.sprites.other.home.front_default" :alt="pokemon.name">
    </div>
    <div class="info">
      <h4>Especie: {{ pokemon.species.name }}</h4>
      <hr>
      <h4>Tipos:</h4>
      <h5 v-for="tipo in pokemon.types">{{ tipo.type.name }}</h5>
      <hr>
      <h4>{{ jaName }}</h4>
    </div>
  </div>
</template>

<style scoped>.nav {
  position: fixed;
  top: 0;
  left: 50%;
  transform: translate(-50%);
  background: white;
  padding: 15px;
  border-radius: 0 0 15px 15px;
  display: flex;
  justify-content: space-around;
  align-items: center;
  width: 100vw;
  flex-flow: row nowrap;
}

.nav img {
  height: 70px;
}

.search {
  display: flex;
  gap: 50px;
  align-items: center;
  justify-self: center;
  align-self: center;
}

.pokeData img {
  width: 400px;
  animation: jump 1s infinite ease-in;
}

@keyframes jump {
  50% {
    transform: translate(0, 10px);
  }
}

.pokeData {
  display: flex;
  justify-content: space-between;
  flex-flow: row wrap;
  align-items: center;
  gap: 100px;
  width: 100vw;
  margin-top: 100px;
}

.info {
  display: flex;
  gap: 5px;
  flex-flow: column nowrap;
  width: 300px;
  background: #d3d3d3b9;
  border-radius: 15px 0 0 15px;
  padding: 10px;
}

.info:first-of-type {
  border-radius: 0 15px 15px 0;

}
</style>

