<template>
  <div v-if="team">
      <PlayerPreview v-for="player in team.player" :key="player.id" :player="player"></PlayerPreview>
      <div v-if="team.games.future_games_arr!=[]">
          <h2>Future Games</h2>  
          <GamePreview v-for="game in team.games.future_games_arr" :key="game.id" :game="game"></GamePreview>
      </div>
      <div v-if="team.games.past_games_arr!=[]">
          <h2>Past Games</h2>
          <PastGame v-for="game in team.games.past_games_arr" :key="game.id" :pastGame="game"></PastGame>
      </div>
  </div>
</template>

<script>
import PlayerPreview from "../components/PlayerPreview.vue";
import GamePreview from "../components/GamePreview.vue";
import PastGame from "../components/PastGame.vue";
export default {
    components:{
        PlayerPreview,GamePreview,PastGame
    },
    data(){
        return{
            team:undefined
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
            let i=5;
            if(this.$route.params.team_name){
                const team=await this.axios.get(`http://localhost:3000/personalPages/teamPageByName/${this.$route.params.team_name}`);
                this.team=team.data;
            }
            else if(this.$route.params.team_id){
                const team=await this.axios.get(`http://localhost:3000/personalPages/teamPage/${this.$route.params.team_id}`);
                this.team=team.data;
            }
        }
    }
}
</script>

<style>

</style>