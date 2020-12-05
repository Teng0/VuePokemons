<template>
  <div class="cards">
    <Card v-for="starter in starters"
          :starter="starter"
          @click="fetchEvolution(starter)"
          :class="{opace:selecedId && selecedId !==starter.id}"
    >
      <template v-slot:title>
        {{starter.name}} #  {{starter.id}}
      </template>
      <template v-slot:content>
        <img :src="starter.sprite" alt="">
      </template>
      <template v-slot:description>
        <div v-for=" type in starter.type">
          {{type}}
        </div>
      </template>
    </Card>
  </div>

  <div class="cards">
    <Card v-for="creature in evolutions">
      <template v-slot:title>
       {{creature.name}} #  {{creature.id}}
      </template>
      <template v-slot:content>
        <img :src="creature.sprite" alt="">
      </template>
      <template v-slot:description>
        <div v-for=" type in creature.type">
          {{type}}
        </div>
      </template>
    </Card>
  </div>
</template>
<script>
  import Card from "./Card.vue"

  const  api="https://pokeapi.co/api/v2/pokemon";
  const STARTER_IDS=[1,4,7]
 export default {
    components:{
      Card
    },
   async created() {
          const  starters = await this.fetch(STARTER_IDS)
          this.starters = starters
    },
   methods:{
      async fetchEvolution(pokemon){
        const  evolution = await this.fetch([pokemon.id+1,pokemon.id+2])
        this.evolutions=evolution
        this.selecedId=pokemon.id
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
img {
  width: 100%;
}
.cards{
  display: flex;
  justify-content: center;
}
.opace{
  opacity: 0.5;
}
.card:hover{
  opacity: 1.0;
}

</style>