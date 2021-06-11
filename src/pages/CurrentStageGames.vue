<template>
  <div >
    <h1 id="title">Games Page</h1>
    <div v-if="!loading">
    <div class="split left">  
      <div id="futureGames" v-if="futureGames">
        <h3 style="margin-left:1%;">Future Games</h3>
          <div v-if="!$root.store.username">
              <FutureGame v-for="g in futureGames" :key="g.id" :game="g"></FutureGame>
          </div>
          <div v-else>
            <FutureGameToAddToFavorites v-for="g in futureGames" :key="g.id" :game="g"></FutureGameToAddToFavorites>
          </div>
      </div>
     <span v-else>No Future Games</span>
      </div>
      <div class="split right">
      <div id="pastGames" v-if="pastGames">
        <h3>Past Games</h3>
        <FutureGame v-for="g in pastGames" :key="g.id" :game="g"></FutureGame>
      </div>
      <span v-else>No Past Games</span>
    </div>
  </div>
  <span v-else id="loading">Loading...</span>
  </div>
</template>

<script>
import FutureGame from "../components/GamePreview.vue";
import FutureGameToAddToFavorites from "../components/GameToAddToFavorites.vue";
export default {
  components:{
    FutureGame,FutureGameToAddToFavorites
  },
  data(){
    return{
      pastGames:undefined,
      futureGames:undefined,
      errorMessage:"No games in database",
      loading:true
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
     this.loading=false;
    }
  }
}
</script>

<style>
 
  #title{
    margin-left: 25%;
    margin-right: 25%;
    margin-top: 1%;
  }
  .split {
  height: 100%;
  width: 50%;
  position: absolute;
  z-index: 1;
  overflow:auto;
}

.left {
  left: 0;
 
}

.right {
  right: 0;
  
}
#loading{
  margin-left: 1%;
  text-align: center;
  font-size: 24px;
  font-weight: bold;
}
</style>