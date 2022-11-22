<script setup lang="ts">
import { ref } from 'vue'
import * as Tone from 'tone'

defineProps<{ msg: string }>()

const count = ref(0)

var context = null;
var oscillator = null;
var isStarted = false;

const synth = new Tone.PolySynth(Tone.Synth).toDestination();
const now = Tone.now()

function getOrCreateContext() {
  if (!context) {
    context = new AudioContext();
    oscillator = context.createOscillator();
    oscillator.connect(context.destination);
  }
  return context;

}

function playSound(frequency, type) {
  getOrCreateContext();
  oscillator.frequency.setTargetAtTime(frequency, context.currentTime, 0);
  if (!isStarted) {
    oscillator.start(0);
    isStarted = true;
  } else {
    context.resume();
  }
  setTimeout(() => { this.stopSound() }, 500 );
}

function stopSound() {
  context.suspend();
}

</script>

<template>

  <div class="flex flex-col h-screen items-center justify-center content-center text-center">

    <h1 v-on:click="synth.triggerAttackRelease('C4', now)"
      className="text-3xl flex flex-1 basis-1/2 items-center justify-center content-center w-screen h-1/2 font-bold underline transition-all ease-in-out delay-150 bg-blue-500 text-white p-1 hover:p-5 hover:text-xl hover:bg-yellow-500">
      <span v-on:click="synth.triggerAttackRelease('D4', now)"
        class="flex items-center justify-center content-center rounded-full inline-block p-3 font-bold underline transition-all ease-in-out delay-150 bg-black min-h-[30%] min-w-[30%] text-white group-hover:text-xl hover:bg-red-500 hover:min-h-[50%] hover:min-w-[50%]">
        {{ msg }}</span>
    </h1>

    <div v-on:click="synth.triggerAttackRelease('E4', now)"
      class="group flex flex-1 items-center basis-1/2  justify-center content-center text-3xl font-bold underline transition-all ease-in-out delay-150 bg-red-500 h-1/2 w-screen hover:text-xl hover:bg-black">
      <button v-on:click="synth.triggerAttackRelease('G4', now)"
        class="text-3xl rounded-full p-3 font-bold underline transition-all ease-in-out delay-150 bg-black min-h-[30%] min-w-[30%] text-white group-hover:text-xl hover:bg-red-500 hover:min-h-[50%] hover:min-w-[50%]"
        type="button" @click="count++">{{ count }}</button>
    </div>
  </div>

</template>

<style scoped>

</style>
