<template>
  <h1 v-if="!pokemon">Espere por favor...</h1>

  <div v-else>
    <h1>Quien es este pokémon?</h1>

    <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon" />
    <PokemonOptions :pokemons="pokemonArr" @selection="checkAnswer" />

    <template v-if="showAnswer">
      <h2 class="fade-in">{{ message }}</h2>
      <button @click="newGame">Siguiente</button>
    </template>
    
  </div>
</template>


<script>
import PokemonPicture from "@/components/PokemonPicture.vue";
import PokemonOptions from "@/components/PokemonOptions.vue";

import getPokemonOptions from "@/helpers/getPokemonsOptions.js";

export default {
  components: {
    PokemonPicture,
    PokemonOptions,
  },
  data() {
    return {
      pokemonArr: [],
      pokemon: null,
      showPokemon: false,
      showAnswer: false,
      message: "",
    };
  },
  methods: {
    async mixPokemonArray() {
      this.pokemonArr = await getPokemonOptions();
      const rndInt = Math.floor(Math.random() * 4);
      this.pokemon = this.pokemonArr[rndInt];
    },
    checkAnswer(pokemonId) {
      this.showPokemon = true;
      this.showAnswer = true;

      if (pokemonId === this.pokemon.id) {
        this.message = `Correcto, ${this.pokemon.name}`;
      } else {
        this.message = `Oops, era ${this.pokemon.name}`;
      }
    },
    newGame() {
      this.showPokemon = false;
      this.showAnswer = false;
      this.pokemonArr = [];
      this.mixPokemonArray();
    },
  },
  mounted() {
    this.mixPokemonArray();
  },
};
</script>