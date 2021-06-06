<template>
  <div>
      <span @click="moveToTeamPage"> Team Name: {{team.team_name}}</span>
      <img :src="team.team_logo" @click="moveToTeamPage">
  </div>
</template>

<script>
export default {
    props:{
        team:Object
    },
    methods:{
        moveToTeamPage(){
            this.$router.push({name:"teamPageById",params:{team_id:this.team.team_id}});
        }
    }
}
</script>

<style>

</style>