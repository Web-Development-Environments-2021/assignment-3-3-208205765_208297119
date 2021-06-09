<template>
  <div>
    <h1 class="title">Search Page</h1>
    <span>Sort Players By:</span>
     <select v-model="sortPlayersBy" @change="sortPlayersArray" >
       <option value=""></option>
       <option value="playerName" >Player Name</option>
       <option value="teamName" >Team Name</option>
   </select>
   <br/>
   <span> Sort Teams By:</span>
   <input type="checkbox" id="checkBox" v-model="sortTeamsBy" value="sortByName" @change="sortTeams">
   <label for="checkBox">Team Name</label>
   <br/>
   <h3>Filters</h3>
   <b-input-group prepend="Player Position"><b-form-input v-model="playerPositionFilter"></b-form-input></b-input-group>
   <b-input-group prepend="Team Name"><b-form-input v-model="playerTeamFilter"></b-form-input></b-input-group>
    <b-input-group><b-button :disabled="isDisabled"  variant="primary" @click="filterPlayers">Filter</b-button></b-input-group>
    <br/>
      <h3>Query</h3>
    <b-input-group prepend="Search Query:" id="search-input">
      <b-form-input v-model="searchQuery"></b-form-input>
     
      <b-input-group-append>
        <b-button variant="success" :disabled="disabledSearchButton" @click="search">Search</b-button>
      </b-input-group-append>
    </b-input-group>
    <br/>
    <p v-if="searching">Searching...</p>
    <div id="results" v-else-if="!searching && pressedSearchButton">
    <h1>Results:</h1>
    <span v-if="playersArray.length==0 && teamsArray.length==0 && searchQuery!='' && !searching">No Results!</span>
    <div v-else>
      <div id="playersDiv" v-if="playersArray.length!=0">
        <h2>Players:</h2>
        <div v-if="sortedAndFilteredArray.length==0">
          <playerPreview v-for="player in playersArray" :key="player.player_id" :player="player"></playerPreview>
        </div>
        <div v-else>
          <playerPreview v-for="player in sortedAndFilteredArray" :key="player.player_id" :player="player"></playerPreview>
        </div>
    </div>
    <div id="teamsDiv" v-if="teamsArray.length!=0">
      <div v-if="!sortTeamsBy">
      <teamPreview v-for="team in teamsArray" :key="team.team_id" :team="team" ></teamPreview></div>
      <div v-else>
        <teamPreview v-for="team in sortedTeamsArray" :key="team.team_id" :team="team"></teamPreview>
      </div>
    </div>
    </div>
    
  </div>
  </div>
</template>

<script>
import playerPreview from "../components/PlayerPreview.vue";
import teamPreview from "../components/TeamPreview.vue";
export default {
  components:{
    playerPreview,teamPreview
  },
 data() {
    return {
      searchQuery:"",
      playersArray:[],
      teamsArray:[],
      coachesArray:[],
      sortPlayersBy:"",
      sortTeamsBy:false,
      filteredPlayersArray:[],
      playerPositionFilter:"",
      playerTeamFilter:"",
      sortedTeamsArray:[],
      sortedPlayersArray:[],
      sortedAndFilteredArray:[],
      searching:false,
      pressedSearchButton:false
    };
  },
  computed:{
    isDisabled(){
      return this.playerPositionFilter=="" && this.playerTeamFilter=="";
    },
    disabledSearchButton(){
      return this.searchQuery=="";
    }
    },
  mounted(){
    try{
      if(this.$root.store.username){
        this.getLastResults();
      }
    }
    catch(error){
      console.log(error);
    }
  },
  methods:{
    async search(){
      if(this.searchQuery!=""){
        this.searching=true;
        this.pressedSearchButton=true;
        const response=await this.axios.get(`http://localhost:3000/search/${this.searchQuery}`);
        this.playersArray=response.data.playersArray;
        this.teamsArray=response.data.teamsArray;
        this.coachesArray=response.data.coachesArray;
        this.sortTeams();
        this.sortPlayersArray();
        this.filterPlayers();
        this.searching=false;
      }
    },
    async getLastResults(){
      const response=await this.axios.get(`http://localhost:3000/search/lastResults`);
      this.playersArray=response.data.playersArray;
      this.coachesArray=response.data.coachesArray;
      this.teamsArray=response.data.teamsArray;
    },
    filterPlayers(){
      if(this.playersArray.length>0){
        let filteredPlayers=[];
        if(this.playerPositionFilter && this.playerTeamFilter){
          for(let i=0;i<this.playersArray.length;i++){
            if(this.playersArray[i].team_name==this.playerTeamFilter && this.playersArray[i].position_number.toString()==this.playerPositionFilter){
              filteredPlayers.push(this.playersArray[i]);
            }
          }
        }
        else if(this.playerPositionFilter){
          for(let i=0;i<this.playersArray.length;i++){
            if(this.playersArray[i].position_number.toString()==this.playerPositionFilter){
              filteredPlayers.push(this.playersArray[i]);
            }
          }
        }
        else if(this.playerTeamFilter){
          for(let i=0;i<this.playersArray.length;i++){
            if(this.playersArray[i].team_name==this.playerTeamFilter){
              filteredPlayers.push(this.playersArray[i]);
            }
          }
        }
        this.filteredPlayersArray=filteredPlayers;
        this.mergeSortedAndFilteredPLayers();
      }
    },
    compareTeams(a,b){
      if(a.team_name<b.team_name){
        return -1;
      }
      if(a.team_name>b.team_name){
        return 1;
      }
      return 0;
    },
    compareByPlayerName(a,b){
        if(a.full_name<b.full_name){
          return -1;
        }
        if(a.full_name>b.full_name){
          return 1;
        }
        return 0;
      },
       compareByTeamName(a,b){
      if(a.team_name<b.team_name){
        return -1;
      }
      if(a.team_name>b.team_name){
        return 1;
      }
      return 0;
    },
    sortTeams(){
      if(this.sortTeamsBy){
        if(this.teamsArray.length!=0){
          this.sortedTeamsArray=[...this.teamsArray];
          this.sortedTeamsArray.sort(this.compareTeams);
      }
      }
      else{
        this.sortedTeamsArray=[];
      }
    },
    sortPlayersArray(){
      if(this.sortPlayersBy==""){
        this.sortedPlayersArray=[];
        this.mergeSortedAndFilteredPLayers();
        return;
      }
      if(this.playersArray.length!=0){
        this.sortedPlayersArray=[...this.playersArray];
        if(this.sortPlayersBy=="playerName"){
        this.sortedPlayersArray.sort(this.compareByPlayerName);
      }
      else{
        this.sortedPlayersArray.sort(this.compareByTeamName);
      }
      }
      this.mergeSortedAndFilteredPLayers();
    },
    mergeSortedAndFilteredPLayers(){
      if(this.filteredPlayersArray.length==0 && this.sortedPlayersArray.length==0){
          this.sortedAndFilteredArray=[];
      }
      else if(this.filteredPlayersArray.length==0){
        this.sortedAndFilteredArray=this.sortedPlayersArray;
      }
      else if(this.sortedPlayersArray.length==0){
        this.sortedAndFilteredArray=this.filteredPlayersArray;
      }
      else{
        let mergedFilteredAndSortedArray=[];
        for( let i=0;i<this.sortedPlayersArray;i++){
          if(this.filteredPlayersArray.includes(this.sortedPlayersArray[i])){
            mergedFilteredAndSortedArray.push(this.sortedPlayersArray[i]);
          }
        }
        this.sortedAndFilteredArray=mergedFilteredAndSortedArray;
      }
    }
  }
}
</script>

<style scoped>

#search-input {
  margin-left: 20px; 
  width: 500px; 
}
</style>