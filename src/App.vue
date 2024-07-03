<template>
  <div>
    <RangeSlider v-model:modelValue="volume" />
    <SoundPlayer />
  </div>
</template>

<script setup>
import { ref } from 'vue';
import RangeSlider from './components/RangeSlider.vue';
import SoundPlayer from './components/SoundPlayer.vue';

const volume = ref(50); 

</script>
