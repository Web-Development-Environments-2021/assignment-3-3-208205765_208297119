<template>
  <div class="container">
    <h1 class="title">Main Page</h1>
    <div v-if="!loading">
    <div id="leftScreen">
    <LeagueInfo></LeagueInfo>
    </div>
    <div id="rightScreen">
    <LoginPage v-if="!$root.store.username"></LoginPage>
    <FavoriteGames v-else-if="favoriteGames" :games="favoriteGames"></FavoriteGames>
    <span style="right:0;" v-else> {{errorMessageForLoggedInUser}}</span>
    </div>
    </div>
    <span v-if="loading">Loading...</span>
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
      loading:true
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
        this.loading=false;
    }
    
    catch(error){
      console.log(error);
    }
    }
  }
};
</script>

<style lang="scss" scoped>
.RandomRecipes {
  margin: 10px 0 10px;
}
.blur {
  -webkit-filter: blur(5px); /* Safari 6.0 - 9.0 */
  filter: blur(2px);
}
::v-deep .blur .recipe-preview {
  pointer-events: none;
  cursor: default;
}
#leftScreen{
  left: 0;
}
#rightScreen{
  right: 0;;
}
</style>
