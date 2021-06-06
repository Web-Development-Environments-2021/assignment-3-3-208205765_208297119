<template>
  <div>
      <b-form @submit.prevent="addGame" @reset.prevent="resetForm">
          <b-form-group label="Home Team" label-for="homeTeamInput">
              <b-form-input id="homeTeamInput" v-model="home_team" type="text" placeholder="Home Team" required></b-form-input></b-form-group>
              <b-form-group label="Away Team" label-for="awayTeamInput">
                  <b-form-input id="awayTeamInput" v-model="away_team" type="text" placeholder="Away Team" required></b-form-input></b-form-group>
          <b-form-group label="Date Of The Match" label-for="dateInput">
              <b-form-datepicker id="dateInput" v-model="date" class="mb-2"></b-form-datepicker></b-form-group>
          <b-form-group label="Time Of The Game" label-for="timeInput">
              <b-form-timepicker v-model="time" locale="en"></b-form-timepicker></b-form-group>
          <b-form-group label="Stadium" label-for="stadiumInput">
              <b-form-input id="stadiumInput" v-model="stadium" type="text" placeholder="Stadium" required></b-form-input></b-form-group>
          <b-form-group label="Referee" label-for="refereeInput">
              <b-form-input v-model="referee" type="text" placeholder="Referee" required></b-form-input></b-form-group></b-form>        

  </div>
</template>

<script>
export default {
    data(){
        return{
            home_team:undefined,
            away_team:undefined,
            date:undefined,
            time:undefined,
            stadium:undefined,
            referee:undefined,
           
        }
    }
}
</script>

<style>

</style>