<template>
  <div class="container">
    <h1 >Main Page</h1>
    
    <div class="leftScreen screen">
    <LeagueInfo @finishLoading="changeLoading" :show="loadingNextGame"></LeagueInfo>
    </div>
    <div class="rightScreen screen" v-if="!loadingFavoriteGames && loadingNextGame">
    <LoginPage  v-if="!$root.store.username"></LoginPage>
    <FavoriteGames v-else-if="favoriteGames" :games="favoriteGames"></FavoriteGames>
    <span  v-else> {{errorMessageForLoggedInUser}}</span>
    </div>
    
    <label v-if="!loadingNextGame || loadingFavoriteGames">Loading...</label>
  </div>
</template>

<script>
import LeagueInfo from "../components/LeagueInfo";
import FavoriteGames from "../components/FavoriteGames";
import LoginPage from "../pages/LoginPage";
export default {
  components: {
    LeagueInfo, 
    LoginPage, 
    FavoriteGames
  },
  data(){
    return{
      errorMessageForLoggedInUser:"No Favorite Games",
      favoriteGames:undefined,
      loadingNextGame:false,
      loadingFavoriteGames:true
    }
  },
  created(){
    this.getFavoriteGames()
  },
  
  methods:{
    async getFavoriteGames(){
      try{
        if(this.$root.store.username){
      this.axios.withCredentials=true;
      const response=await this.axios.get("http://localhost:3000/mainPage/rightColumn",{withCredentials:true});
      if(response.status==204){
        this.favoriteGames=undefined;
      }
      else if(response.status==200){
        this.favoriteGames=response.data;
      }
        }
        this.loadingFavoriteGames=false;
    }
    
    catch(error){
      console.log(error);
    }
    },
    changeLoading(){
      while(this.loadingFavoriteGames){}
      this.loadingNextGame=true;
      }
  }
};
</script>

<style >
.container{
  background-image: url('../assets/1200px-Superliga_2010.svg.png');
  
 }
  
 .screen{
   height: 100%;
   width: 50%;
   position: fixed;
   z-index: 1;
 }
 .leftScreen{
   left: 0;
   margin-left: 1%;
 }
 .rightScreen{
   right: 0;
 }
</style>
