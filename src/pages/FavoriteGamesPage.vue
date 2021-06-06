<template>
  <div>
      <FavoriteGames v-if="games" :games="games"></FavoriteGames>
      <span v-else> {{errorMessage}}</span>
  </div>
</template>

<script>
import FavoriteGames from "../components/FavoriteGames.vue";
export default {
    components:{
        FavoriteGames
    },data(){
        return{
            games:undefined,
            errorMessage:"You don't have favorite games"
        }
    },
    mounted(){
        try{
            this.getFavoriteGames();
        }
        catch(error){
            console.log(error);
        }
    },
    methods:{
        async getFavoriteGames(){
            const response=await this.axios.get(`http://localhost:3000/users/getFavoriteGames`);
            if(response.status==200){
                this.games=response.data;
            }
        }
    }
}
</script>

<style>

</style>