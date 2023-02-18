<template>
  <h1 v-if="!pokemon">Espere por favor...</h1>
  <div v-else>
    <h1>Quién es este pokémon?</h1>
    <PokemonPicture
      :pokemon-id="pokemon.id"
      :show-pokemon="showPokemon"
    />
    <PokemonOptions
      :pokemons="pokemonArr"
      @selection="checkRes"
    />

    <template v-if="showRes">
      <h2>{{msg}}</h2>
      <button
        @click="newGame"
      >Nuevo Juego</button>
    </template>
  </div>
</template>

<script>
import PokemonOptions from '@/components/PokemonOptions.vue'
import PokemonPicture from '@/components/PokemonPicture.vue'

import getPokemonOptions from '@/helpers/getPokemonOptions';

export default {
  components: { 
    PokemonOptions, 
    PokemonPicture 
},
data(){
  return {
    pokemonArr: [],
    pokemon: null,
    showPokemon: false,
    showRes: false,
    msg: '',
  }
},
methods: {
  async mixPokemonArray(){
    this.pokemonArr = await getPokemonOptions()
    
    const rndInt = Math.floor(Math.random() * 4)
    this.pokemon = this.pokemonArr[rndInt]
  },
  checkRes(selectedId){
    this.showPokemon = true
    this.showRes = true

    if(selectedId === this.pokemon.id){
      this.msg = `Ah, perro! Traes el omnitrix. El Pokemon sí es:${this.pokemon.name}`
    }else {
      this.msg = `Oops, no le atinaste. El pipeco era ${this.pokemon.name}`
    }
  },
  newGame(){
    this.pokemonArr = []
    this.pokemon = null
    this.showPokemon = false
    this.showRes = false
    this.mixPokemonArray()
  }
},
mounted(){
  this.mixPokemonArray()
}

}
</script>

