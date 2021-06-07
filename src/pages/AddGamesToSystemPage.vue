<template>
  <div>
      <b-form @submit.prevent="validateForm" @reset.prevent="resetForm">
          <b-form-group label="Home Team" label-for="homeTeamInput">
              <b-form-input id="homeTeamInput" v-model="$v.form.home_team.$model" type="text" placeholder="Home Team" :state="validateState('home_team')"></b-form-input>
              <b-form-invalid-feedback v-if="!$v.form.home_team.required">Home team must be provided</b-form-invalid-feedback>
              <b-form-invalid-feedback v-if="!$v.form.home_team.alpha">Home team name must contain only letters</b-form-invalid-feedback>
              </b-form-group>
              <b-form-group label="Away Team" label-for="awayTeamInput">
                  <b-form-input id="awayTeamInput" v-model="$v.form.away_team.$model" type="text" placeholder="Away Team" :state="validateState('away_team')"></b-form-input>
                  <b-form-invalid-feedback v-if="!$v.form.away_team.required">Away team must be provided</b-form-invalid-feedback>
                  <b-form-invalid-feedback v-if="!$v.form.away_team.alpha">Away team name must contain only letters</b-form-invalid-feedback>
                  </b-form-group>
          <b-form-group label="Date Of The Match" label-for="dateInput">
              <b-form-datepicker id="dateInput" locale="en-US" v-model="$v.form.date.$model" :State="validateState('date')" class="mb-2"></b-form-datepicker>
              <b-form-invalid-feedback v-if="!$v.form.date.required">Date must be provided</b-form-invalid-feedback>
              </b-form-group>
          <b-form-group label="Time Of The Game" label-for="timeInput">
              <b-form-timepicker v-model="$v.form.time.$model" locale="en-US" :state="validateState('time')"></b-form-timepicker>
              <b-form-invalid-feedback v-if="!$v.form.time.required"> Time of the game must be provided</b-form-invalid-feedback>
              </b-form-group>
          <b-form-group label="Stadium" label-for="stadiumInput">
              <b-form-input id="stadiumInput" v-model="$v.form.stadium.$model" type="text" placeholder="Stadium" :state="validateState('stadium')"></b-form-input>
              <b-form-invalid-feedback v-if="!$v.form.stadium.required">Stadium name must be provided</b-form-invalid-feedback>
              <b-form-invalid-feedback v-if="!$v.form.stadium.alpha">Stadium name must contain only letters</b-form-invalid-feedback>
              </b-form-group>
          <b-form-group label="Referee" label-for="refereeInput">
              <b-form-input v-model="$v.form.referee.$model" type="text" placeholder="Referee" :state="validateState('referee')"></b-form-input>
              <b-form-invalid-feedback v-if="!$v.form.referee.required">Referee name must be provided</b-form-invalid-feedback>
              <b-form-invalid-feedback v-if="!$v.form.referee.alpha">Referee name must contain only letters</b-form-invalid-feedback>
              </b-form-group>
              <b-button type="submit" variant="primary">Submit</b-button>
             <b-button type="reset" variant="danger">Reset</b-button>
              </b-form>        
            <b-alert v-if="form.success" variant="success" show="">{{form.success}}</b-alert>
            <b-alert v-else-if="form.errorMessage" variant="danger" show="">{{form.errorMessage}}</b-alert>
  </div>
</template>

<script>
import{required,alpha} from "vuelidate/lib/validators";
export default {
    data(){
        return{
            form:{
            home_team:undefined,
            away_team:undefined,
            date:undefined,
            time:undefined,
            stadium:undefined,
            referee:undefined,
            errorMessage: undefined,
            success:undefined
            }
        }
    },
    validations:{
        form:{
            home_team:{
                required,alpha
            },
            away_team:{
                required,alpha
            },
            date:{
                required
            },
            time:{
                required
            },
            stadium:{
                required,alpha
            },
            referee:{
                required,alpha
            }
        }
    },
    methods:{
      validateState(param) {
        const { $dirty, $error } = this.$v.form[param];
        return $dirty ? !$error : null;
        },
        async addGame(){
            try{
                let date_and_time=this.form.date+" "+this.form.time;
                const response=await this.axios.post(`http://localhost:3000/leagueManagment/addGameToSystem`,{
                    home_team:this.form.home_team,
                    away_team:this.form.away_team,
                    date_and_time:date_and_time,
                    stadium:this.form.stadium,
                    referee:this.form.referee
                });
                if(response.status==201){
                      this.form.success="Game was successfully added!";  
                }
                else if(response.status==409){
                    this.form.errorMessage="Game was already added to the system";
                }
            }
            catch(error){
                this.form.errorMessage=error.response.data;
            }
        },
        validateForm(){
             this.$v.form.$touch();
            if (this.$v.form.$anyError) {
                return;
            }
            this.addGame();
        },
        resetForm(){
            this.form={
                home_team:undefined,
                away_team:undefined,
                date:undefined,
                time:undefined,
                stadium:undefined,
                referee:undefined,
                alertMessage:undefined
            };
            this.$nextTick(() => {
                this.$v.$reset();
        });
        }
    }
}
</script>

<style>

</style>