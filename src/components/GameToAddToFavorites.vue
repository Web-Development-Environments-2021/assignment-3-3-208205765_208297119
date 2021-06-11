<template>
  <div>
      <GamePreview :game="game"></GamePreview>
      <b-button style="margin-left: 1%; margin-top:0.5%;margin-bottom:0.5%" variant="outline-primary" :click="addToFavorites">Add To Favorites </b-button>
  </div>
</template>

<script>
import GamePreview from "./GamePreview.vue";
export default {
components:{
    GamePreview
},
props:{
    game:Object
},
methods:{
    async addToFavorites(){
        try{
            await this.axios.post(`http://localhost:3000/users/addGameToFavorites/${this.game.id}`);
        }
        catch(error){
            console.log(error);
        }
    }
}
}
</script>

<style>
   
</style>