<template>
  <p>Puntaje: {{ puntaje }}</p>
  <p>Intento: {{ intento }}</p>
  <div class="contenedor">
    <div class="pokemon">
      <Pokemon/>
    </div>
    <div class="pokemon">
      <Pokemon/>
    </div>
    <div class="pokemon">
      <Pokemon/>
    </div>
  </div>
  <button @click="juagr">Jugar</button>
</template>

<script>
import Pokemon from "@/components/Pokemon.vue";
export default {
  components: {
    Pokemon,
  },
  data() {
    return {
      pokemon1: null,
      pokemon2: null,
      pokemon2: null,
      listaPokemon: [
        {
          img: APIPokemonImagen(10),
          nombre: APIPokemonNombre(10),
        },
        {
          img: APIPokemonImagen(10),
          nombre: APIPokemonNombre(10),
        },
        {
          img: APIPokemonImagen(10),
          nombre: APIPokemonNombre(10),
        },
        {
          img: APIPokemonImagen(10),
          nombre: APIPokemonNombre(10),
        },
        {
          img: APIPokemonImagen(10),
          nombre: APIPokemonNombre(10),
        },
      ],
      mostrar: false,
      puntaje: 0,
      intento: 0,
    };
  },
  methods: {
    pokemonAleatorio() {
      return Math.floor(Math.random() * 5);
    },
    async APIPokemonImagen(idPokemon) {
      const pokemon = await fetch(
        "https://pokeapi.co/api/v2/pokemon/" + idPokemon
      ).then((r) => r.json());
      return pokemon.sprites.other.dream_world.front_default;
    },
    async APIPokemonNombre(idPokemon) {
      const pokemon = await fetch(
        "https://pokeapi.co/api/v2/pokemon/" + idPokemon
      ).then((r) => r.json());
      return pokemon.forms[0].name;
    },
  },
  juagr() {
    this.mostrar = true;
    this.pokemon1 = this.listaPokemon[pokemonAleatorio]
    this.pokemon2 = this.listaPokemon[pokemonAleatorio]
    this.pokemon3 = this.listaPokemon[pokemonAleatorio]
    if(this.pokemon1 == this.pokemon2 == this.pokemon3){
        this.puntaje+=5
    }
  },
};
</script>

<style>
.contenedor {
  display: flex;
  justify-content: center;
}
.cortina {
  flex-grow: 1;
  background-color: black;
  margin: 50px;
}
button {
  width: 300px;
  padding: 10px;
  font-size: large;
}
</style>