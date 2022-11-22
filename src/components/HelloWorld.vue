<script setup lang="ts">
import { ref } from 'vue'

defineProps<{ msg: string }>()

const count = ref(0)

var context = null;
var oscillator = null;
function getOrCreateContext() {
  if (!context) {
    context = new AudioContext();
    oscillator = context.createOscillator();
    oscillator.connect(context.destination);
  }
  return context;
  
}

        var isStarted = false;
        function playSound(frequency, type) {
          getOrCreateContext();
          oscillator.frequency.setTargetAtTime(frequency, context.currentTime, 0);
          if (!isStarted) {
            oscillator.start(0);
            isStarted = true;
          } else {
            context.resume();
          }
	  setTimeout(context.suspend, 500);
        }

        function stopSound() {
          context.suspend();
        }

        document.getElementById('basic').addEventListener('click', playSound.bind(null, 440, 'square'));
        document.getElementById('basic2').addEventListener('click', playSound.bind(null, 800, 'square'));
        document.getElementById('basic3').addEventListener('click', playSound.bind(null, 1000, 'square'));
        document.getElementById('stop').addEventListener('click', stopSound);


</script>

<template>

<div class="flex flex-col h-screen items-center justify-center content-center text-center">

  <h1 className="text-3xl flex flex-1 basis-1/2 items-center justify-center content-center w-screen h-1/2 font-bold underline transition-all ease-in-out delay-150 bg-blue-500 text-white p-1 hover:p-5 hover:text-xl hover:bg-yellow-500">
  <span class="flex items-center justify-center content-center rounded-full inline-block p-3 font-bold underline transition-all ease-in-out delay-150 bg-black min-h-[30%] min-w-[30%] text-white group-hover:text-xl hover:bg-red-500 hover:min-h-[50%] hover:min-w-[50%]" >
  {{ msg }}</span>
  </h1>

  <div class="group flex flex-1 items-center basis-1/2  justify-center content-center text-3xl font-bold underline transition-all ease-in-out delay-150 bg-red-500 h-1/2 w-screen hover:text-xl hover:bg-black">
    <button class="text-3xl rounded-full p-3 font-bold underline transition-all ease-in-out delay-150 bg-black min-h-[30%] min-w-[30%] text-white group-hover:text-xl hover:bg-red-500 hover:min-h-[50%] hover:min-w-[50%]" type="button" @click="count++">{{ count }}</button>
  </div>
  </div>

</template>

<style scoped>

</style>
