
<script setup>
import {computed, ref} from "vue";
    function getNextMealTime() {
    let date = new Date();
    let hours = date.getHours();

    if (hours < 7){
    date.setHours(7, 0, 0, 0)
} else if(hours >= 7 &&  hours < 12){
    date.setHours(12,0,0,0)
}  else if (hours >= 12 <19){
      date.setHours(19,0,0,0)
    }
    else if(hours >=19) {
    date.setDate(date.getDate() + 1);
    // Set the time to 7am tomorrow
    date.setHours(7, 0, 0, 0);
}
    return date.getTime();
}

    console.log(getNextMealTime(), "next meal");
    const currentTime = ref(new Date().getTime());
    const timeDiff = (getNextMealTime() - currentTime.value);

    const waitingPeriod = computed(()=>{
    const hours = Math.floor((timeDiff)/ (1000 * 60 * 60));
    const minutes =  Math.floor((timeDiff % 3600000) / (1000 * 60))
    return{
    hours, minutes
}
})
    console.log(timeDiff);
</script>
<template>
  <div>
    <H1>Jake's Meal Time</H1>
    <p> [{{waitingPeriod.hours}}, {{waitingPeriod.minutes}}] </p>
  </div>
</template>