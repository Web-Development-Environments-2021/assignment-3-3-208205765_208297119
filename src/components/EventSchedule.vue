<template>
  <div>
      <Event v-for="(e,index) in events" :key="index" :event="e"></Event>
  </div>
</template>

<script>
import Event from "./Event.vue";
export default {
    components:{
        Event
    },
    props:{
        eventsArr:Array
    },
    data(){
        return{
            events:this.eventsArr
        }
    }
}
</script>

<style>

</style>