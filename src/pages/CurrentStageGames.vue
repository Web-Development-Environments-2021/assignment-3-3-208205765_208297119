<template>
  <div id="mainDiv">
      <div id="futureGames" v-if="futureGames">
          <div v-if="$root.store.username">
              <FutureGame v-for="g in futureGames" :key="g.id" :game="g"></FutureGame>
          </div>
          <div v-else>
            <FutureGameToAddToFavorites v-for="g in futureGames" :key="g.id" :game="g"></FutureGameToAddToFavorites>
          </div>
      </div>
      <div id="pastGames" v-if="pastGames">
        <PastGame v-for="g in pastGames" :key="g.id" :pastGame="g"></PastGame>
      </div>
  </div>
</template>

<script>
import FutureGame from "../components/GamePreview.vue";
import PastGame from "../components/PastGame.vue";
import FutureGameToAddToFavorites from "../components/GameToAddToFavorites.vue";
export default {
  components:{
    FutureGame,PastGame,FutureGameToAddToFavorites
  },
  data(){
    return{
      pastGames:undefined,
      futureGames:undefined,
      errorMessage:"No games in database"
    }
  },
  mounted(){
    try{
      this.getGames();
    }
    catch(error){
      console.log(error);
    }
  },
  methods:{
    async getGames(){
      const response=await this.axios.get("http://localhost:3000/currentStageGames");
      if(response.status==200){
        if(response.data.past_games_arr!=[]){
          this.pastGames=response.data.past_games_arr;
        }
        if(response.data.future_games_Arr!=[]){
          this.futureGames=response.data.future_games_arr;
        }
      }
     
    }
  }
}
</script>

<style>
  #mainDiv{
    display: flexbox;
    flex-direction: row;
  }
</style>