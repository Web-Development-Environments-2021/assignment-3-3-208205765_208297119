<template>
  <div>
      <GamePreview :game="game"></GamePreview>
      <button :click="addToFavorites">Add To Favorites </button>
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