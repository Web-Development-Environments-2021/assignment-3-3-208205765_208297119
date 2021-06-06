<template>
  <div>
      <GamePreview :game="pastGame"></GamePreview>
      <ul>
          <li> Result: {{result}}</li>
          </ul>
      <EventSchedule :eventsArr="eventSchedule"></EventSchedule>    
  </div>
</template>

<script>
import GamePreview from "./GamePreview.vue";
import EventSchedule from "./EventSchedule.vue";
export default {
components:{
    GamePreview,
    EventSchedule
},
props:{
    pastGame:Object
},
data(){
    return{
        result:this.pastGame.result,
        eventSchedule:this.pastGame.events_schedule
    }
}
}
</script>

<style>

</style>