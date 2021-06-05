<template>
  <div>
      <Event v-for="(e,index) in eventsArr" :key="index"></Event>
  </div>
</template>

<script>
import Event from "./Event.vue";
export default {
    components:{
        Event
    },
    props:{
        eventsArr:Object
    },
    data(){
        return{
            eventsArr:this.eventsArr
        }
    }
}
</script>

<style>

</style>