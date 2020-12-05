<template>
  <div class="cards">
    <Card v-for="pokemon in pokemons"
          @click="clicked(pokemon)"
          :class="{opace:selecedId && selecedId !==pokemon.id}"
    >
      {{pokemons}}
      <template v-slot:title>
        {{pokemon.name}} #  {{pokemon.id}}
      </template>
      <template v-slot:content>
        <img :src="pokemon.sprite" alt="">
      </template>
      <template v-slot:description>
        <div v-for=" type in pokemon.type">
          {{type}}
        </div>
      </template>
    </Card>
  </div>
</template>
<script>
import Card from "./Card.vue"
export default {
  components:{
    Card
  },
  props:{
    pokemons:{type:Array},
    selecedId:{type:Number}
  },
  methods:{
    clicked(pokemon){
      this.$emit("pokemonClicked",pokemon)
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