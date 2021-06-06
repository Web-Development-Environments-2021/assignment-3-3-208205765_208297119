<template>
  <div class="game-preview">
    
    <ul class="game-content">
      <li @click="moveToTeamPage(hostTeam)"> host: {{ hostTeam }}</li>
      <li @click="moveToTeamPage(guestTeam)"> guest: {{ guestTeam }}</li>
      <li> date: {{ date }}</li>
      <li> hour: {{ hour }}</li>
      <li> stadium: {{stadium}}</li>
      <li> referee: {{referee_name}} </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "GamePreview",
  props: {
      game: Object
  }, 
  data(){
    return{
      hostTeam:this.game.home_team,
      guestTeam:this.game.away_team,
      date:this.game.date,
      hour:this.game.hour,
      stadium:this.game.stadium,
      referee_name:this.game.referee_name
    };

  },
  mounted(){
    console.log("game preview mounted");
    
  },
  methods:{
    moveToTeamPage(team_name){
      this.$router.push({name:"teamPageByName",params:{team_name:team_name}});
    }
  } 
};
</script>

<style>
.game-preview {
  display: inline-block;
  width: 250px;
  height: 200px;
  position: relative;
  margin: 10px 10px;
  border-style: solid;
  border-radius: 10px;
  border-width: 5px;
  border-color:cadetblue;
}

.game-preview .game-title {
  text-align: center;
  text-transform: uppercase;
  color:  rgb(111, 197, 157);
}

.game-preview .game-content {
  width: 100%;
  overflow: hidden;
}



</style>
