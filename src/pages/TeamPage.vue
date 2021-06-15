<template>
  <div id="teamPageDiv" >
      <h1 style="text-align:Center">Team Page</h1>
      <div id="teamDiv" v-if=!loading>
          <div class="playersDiv splitScreen">
       <h2>Players</h2>   
      <PlayerPreview v-for="player in team.players" :key="player.player_id" :player="player"></PlayerPreview>
          </div>
      <div  class="futureGamesDiv splitScreen">
          <h2>Future Games</h2>  
          <GamePreview v-for="game in team.games.future_games_arr" :key="game.id" :game="game"></GamePreview>
          <p v-if="team.games.future_games_arr.length==0">There are no future games</p>
      </div>
      <div class="pastGamesDiv splitScreen">
          <h2>Past Games</h2>
          <GamePreview v-for="game in team.games.past_games_arr" :key="game.id" :game="game"></GamePreview>
          <p v-if="team.games.past_games_arr.length==0">There are no past games</p>
      </div>
      </div>
      <span id="loadingMessage" v-else>Loading...</span>
  </div>
</template>

<script>
import PlayerPreview from "../components/PlayerPreview.vue";
import GamePreview from "../components/GamePreview.vue";
export default {
    components:{
        PlayerPreview,GamePreview
    },
    data(){
        return{
            team:undefined,
            loading:true
        }
    },
    mounted(){
        try{
            this.getTeam();
        }
        catch(error){
            console.log(error);
        }
    },
    methods:{
        async getTeam(){
            if(this.$route.params.team_name){//get team by name
                const team=await this.axios.get(`http://localhost:3000/personalPages/teamPageByName/${this.$route.params.team_name}`);
                this.team=team.data;
            }
            else if(this.$route.params.team_id){//get team by id
                const team=await this.axios.get(`http://localhost:3000/personalPages/teamPage/${this.$route.params.team_id}`);
                this.team=team.data;
            }
            this.loading=false;
        }
    }
}
</script>

<style>
.splitScreen{
    width: 100%;
    overflow: auto;
    z-index: 1;
   
}



#loadingMessage{
    font-size: 24px;
    margin-left: 1%;
}

#teamDiv{
    display: flex;
    justify-content: space-between;
}
#teamPageDiv{
    width: 100%;
    height: 100%;
    color: darkgrey;
    
 
}

</style>