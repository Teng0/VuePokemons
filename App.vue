<template>
  <PokemonCards :pokemons="starters"
                @pokemonClicked="fetchEvolution"
                :selecedId="selecedId"
  > </PokemonCards>
  <PokemonCards :pokemons="evolutions"
                @pokemonClicked="fetchEvolution"
  > </PokemonCards>

</template>
<script>
  import Card from "./Card.vue"
  import PokemonCards from "./PocemonCards.vue"

  const  api="https://pokeapi.co/api/v2/pokemon";
  const STARTER_IDS=[1,4,7]
 export default {
    components:{
      Card,PokemonCards
    },
   async created() {
          const  starters = await this.fetch(STARTER_IDS)
          this.starters = starters
    },
   methods:{
      async fetchEvolution(pokemon){
       this.selecedId = pokemon.id
         const  evolution = await this.fetch([pokemon.id+1,pokemon.id+2])
         this.evolutions=evolution
         //this.selecedId=pokemon.id
      },
     async fetch(ids){
        const responses = await Promise.all(ids.map( id=>window.fetch(`${api}/${id}`)) )
        const data = await Promise.all(responses.map(res=>res.json()))
      return  data.map(datum=>({
         id:datum.id,
         name:datum.name,
         sprite:datum.sprites.other["official-artwork"].front_default,
         type:datum.types.map(type=>(type.type.name))
       }))
     }
   },
  data(){
    return{
      starters:[],
      evolutions:[],
      selecedId:null
    }
  }
 }
</script>
<style scoped>


</style>