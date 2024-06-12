<script setup>
import CountdownHeader from "@/components/CountdownHeader.vue";
import CountdownSegment from "@/components/CountdownSegment.vue";
import {computed, onMounted, onUnmounted, ref, toRaw} from "vue";
import JakesMealTime from "@/components/JakesMealTime.vue";

const secondsLeft = ref(null);
const data = ref(null);

const prepData = () => {
  const today = new Date();
  const nextYear = new Date(new Date().getFullYear() + 1, 0, 1)
  secondsLeft.value = Math.floor((nextYear - today) / 1000); //seconds
  return secondsLeft.value;
};
const difference = computed(()=>{
  const days = Math.floor(secondsLeft.value / 86400);
  const hours = Math.floor((secondsLeft.value % 86400) / 3600);
  const mins = Math.floor((secondsLeft.value % 3600) / 60);
  const sec = secondsLeft.value % 60;
  return {
    days,
    hours,
    mins,
    sec
  }
});

onMounted(()=> {
  data.value = setInterval(prepData, 1000);
})

onUnmounted(()=>{
  clearInterval(data.value);
})


</script>
<template>
  <div class="app-wrapper">
    <div class="countdown-box" v-if="secondsLeft > 0">
      <CountdownHeader />
      <main class="flex justify-center">
        <CountdownSegment data-test="days" label="days" :number="difference.days" />
        <CountdownSegment data-test="hours" label="hours" :number="difference.hours" />
        <CountdownSegment data-test="minutes" label="minutes" :number="difference.mins" />
        <CountdownSegment data-test="seconds" label="seconds" :number="difference.sec" />
      </main>
      <JakesMealTime/>
    </div>
  </div>
</template>

<style scoped>
.app-wrapper {
  @apply flex items-center justify-center w-full h-full p-10;
}
.countdown-box {
  @apply shadow-md relative bg-white p-5 rounded-lg border-gray-100 border-[1px];
}
body {
  @apply bg-gray-100;
}
</style>
