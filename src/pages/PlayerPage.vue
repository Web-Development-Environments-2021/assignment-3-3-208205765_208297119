<template>
  <div>
      <PlayerPreview :player="player"></PlayerPreview>
      <span>Common Name: {{player.common_name}}</span>
      <span>Nationality: {{player.nationality}}</span>
      <span> Birth Date: {{player.birth_date}}</span>
      <span>Birth Country: {{player.birth_country}}</span>
      <span v-if="player.height"> Height: {{player.height}}</span>
      <span v-if="player.weight">Weight: {{player.weight}}</span>
  </div>
</template>

<script>
import PlayerPreview from "../components/PlayerPreview.vue";
export default {
    components:{
        PlayerPreview
    },
    data(){
        return{
            player:undefined
        }
    },
    mounted(){
        try{
            this.getPlayer();
        }
        catch(error){
            console.log(error);
        }
    },
    methods:{
        async getPlayer(){
            const response=await this.axios.get(`http://localhost:3000/personalPages/playerPage/${this.$route.params.player_id}`);
            this.player=response.data;
        }
    }
}
</script>

<style>

</style>