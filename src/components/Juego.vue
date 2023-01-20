<template>
  <div class="contenedor-juego" v-if="mostrarJuego">
    <h1>Juego</h1>
    <div>
      <p>Puntaje: {{ puntaje }}</p>
      <p>Intento: {{ intento }}</p>
    </div>
    <div>
      <img v-bind:src="pokemon1.ruta" alt="NaN" />
      <img v-bind:src="pokemon2.ruta" alt="NaN" />
      <img v-bind:src="pokemon3.ruta" alt="NaN" />
    </div>
    <div>
      <p>{{ pokemon1.nombre }}</p>
      <p>{{ pokemon2.nombre }}</p>
      <p>{{ pokemon3.nombre }}</p>
    </div>
    <button v-on:click="jugar">Jugar</button>
  </div>
  <div class="perdedor" v-if="mostrarPerdedor">
    <h1>Haz utilizado tus 5 intentos</h1>
    <h2>El juego ha terminado, intentalo nuevamente</h2>
    <button v-on:click="nuevoJuego">Nuevo Juego</button>
  </div>
  <div class="ganador" v-if="mostrarGanador">
    <h1>Puntaje: {{ puntaje }}</h1>
    <img src="../img/congratulations.gif" alt="NaN">
    <h2>Felicitaciones has ganado un premio de $10.000,00</h2>
    <button v-on:click="nuevoJuego">Nuevo Juego</button>
  </div>
</template>

<script>

export default {
    data(){
        return {
            puntaje: 0,
            intento: 0,
            banderaJuego: true,
            /*pokemonTexto1: 'xxxxxxxxxxxxxxx',
            pokemonTexto2: 'xxxxxxxxxxxxxxx',
            pokemonTexto3: 'xxxxxxxxxxxxxxx',*/
            pokemon1: {
                ruta: 'https://png.pngtree.com/thumb_back/fh260/background/20200821/pngtree-pure-black-dark-background-wallpaper-image_396553.jpg',
                nombre: 'xxxxxxxxxxxxxxx'
            },
            pokemon2: {
                ruta: 'https://png.pngtree.com/thumb_back/fh260/background/20200821/pngtree-pure-black-dark-background-wallpaper-image_396553.jpg',
                nombre: 'xxxxxxxxxxxxxxx'
            },
            pokemon3: {
                ruta: 'https://png.pngtree.com/thumb_back/fh260/background/20200821/pngtree-pure-black-dark-background-wallpaper-image_396553.jpg',
                nombre: 'xxxxxxxxxxxxxxx'
            },
            mostrarInicio: true,
            mostrarJuego: true,
            mostrarGanador: false,
            mostrarPerdedor: false
        }
    },
    methods: {
        async jugar(){
            const numAl1 = this.obtenerAleatorio(3)
            const numAl2 = this.obtenerAleatorio(3)
            const numAl3 = this.obtenerAleatorio(3)
            const vectorPlantilla = await this.construirPokemons()
            this.pokemon1 = vectorPlantilla[numAl1]
            this.pokemon2 = vectorPlantilla[numAl2]
            this.pokemon3 = vectorPlantilla[numAl3]
            /*this.pokemonTexto1 = this.pokemon1.nombre
            this.pokemonTexto2 = this.pokemon2.nombre
            this.pokemonTexto3 = this.pokemon3.nombre*/
            if(this.intento <= 5){
                this.intento++
                if(this.pokemon1.nombre == this.pokemon2.nombre && this.pokemon1.nombre == this.pokemon3.nombre && this.pokemon2.nombre == this.pokemon3.nombre)
                    this.puntaje += 5
                else if (this.pokemon1.nombre == this.pokemon2.nombre || this.pokemon1.nombre == this.pokemon3.nombre || this.pokemon2.nombre == this.pokemon3.nombre)
                    this.puntaje += 2
            }
            if(this.intento == 5) {
                this.mostrarInicio = false
                this.mostrarJuego = false
                if(this.puntaje >= 10) {
                    this.mostrarGanador = true
                } else {
                    this.mostrarPerdedor = true
                }
            }
            
            
        },
        obtenerAleatorio(limite){
            return Math.floor(Math.random()*limite) + 1
        },
        async consumirAPI(id){
            const data = await fetch('https://pokeapi.co/api/v2/pokemon/'+ id).then((r)=>r.json())
            return data
        },
        definirPokemons(){
            const vectorIdPokemons = [10,20,30,40]
            return vectorIdPokemons
        },
        async construirPokemons(){
            const vectorObjetosPokemons = []
            const vector = this.definirPokemons()
            for(let i = 0; i<4; i++){
                const idPokemon = vector[i]
                const objetoPokemon = await this.construirObjetoPokemon(idPokemon)
                vectorObjetosPokemons.unshift(objetoPokemon)
            }
            return vectorObjetosPokemons
        },
        async construirObjetoPokemon(id){
            const {name} = await this.consumirAPI(id)
            const objetoPkemon = {
                ruta: 'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/' + id + '.svg',
                nombre: name
            }
            return objetoPkemon
        },
        nuevoJuego(){
            this.mostrarInicio = true
            this.mostrarJuego = true
            this.mostrarGanador = false
            this.mostrarPerdedor = false
            this.pokemon1 =  {
                ruta: 'https://png.pngtree.com/thumb_back/fh260/background/20200821/pngtree-pure-black-dark-background-wallpaper-image_396553.jpg',
                nombre: 'xxxxxxxxxxxxxxx'
            }
            this.pokemon2 =  {
                ruta: 'https://png.pngtree.com/thumb_back/fh260/background/20200821/pngtree-pure-black-dark-background-wallpaper-image_396553.jpg',
                nombre: 'xxxxxxxxxxxxxxx'
            }
            this.pokemon3 =  {
                ruta: 'https://png.pngtree.com/thumb_back/fh260/background/20200821/pngtree-pure-black-dark-background-wallpaper-image_396553.jpg',
                nombre: 'xxxxxxxxxxxxxxx'
            }
            this.puntaje = 0
            this.intento = 0
        
        }
    }
}
</script>

<style>
img {
  margin: 0px 20px;
  width: 250px;
  height: 250px;
}
p {
  display: inline;
  margin: 15px 90px;
}
button {
  margin-top: 90px;
}
.perdedor h1 {
    color: red;
}
.perdedor h2 {
    color: red;
}
.ganador h1,h2 {
    color: blue;
}
.ganador img {
    width: auto;
    height: auto;
}
</style>