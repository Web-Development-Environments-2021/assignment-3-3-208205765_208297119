<template>
  <div>
      <span @click="moveToPlayerPage"> Full Name: {{fullName}}</span>
      <span>Team Name: {{teamName}}</span>
      <span>Position Number: {{positionNumber}}</span>
      <img :src="profile_pic" height="200" width="300" @click="moveToPlayerPage">
      </div>
</template>

<script>
export default {
    props:{
        player:Object
    },
    data(){
        return{
            fullName:this.player.full_name,
            teamName:this.player.team_name,
            profile_pic:this.player.pic,
            positionNumber:this.player.position_number
        }
    },
    methods:{
        moveToPlayerPage(){
            this.$router.push(`/playerPage/${this.player.player_id}`);
        }
    }
}
</script>

<style>

</style>