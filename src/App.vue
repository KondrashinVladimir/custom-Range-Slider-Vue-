<template>
  <div>
    <div>
      <button @click="playSound">Воспроизвести звук</button>
    </div>
    <RangeSlider @changeVolumeValue="changeVolumeValue" v-model="volumeValue" />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import RangeSlider from './components/RangeSlider.vue';
import { Howl } from 'howler';
import Sound from './assets/sound.wav';

const volumeValue = ref(50); 
let sound = null;

const changeVolumeValue = (volume) => {
  volumeValue.value = volume;
  if (sound) {
    sound.volume(volumeValue.value / 100); 
  }
};

onMounted(() => {
  sound = new Howl({
    src: [Sound],
    volume: volumeValue.value / 100 
  });
});

const playSound = () => {
  sound.play();
};
</script>
