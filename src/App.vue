<template>
  <div>
    <div>
      <button @click="playSound">Воспроизвести звук</button>
    </div>
    <RangeSlider v-model="volumeValue">
      <template v-slot:right>
        <MyButton class="my-button my-button--volume-on" @click="moveSliderRight"></MyButton>
      </template>
      <template v-slot:left>
        <MyButton class="my-button my-button--volume-off" @click="moveSliderLeft"></MyButton>
      </template>
    </RangeSlider>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue';
import RangeSlider from './components/RangeSlider.vue';
import { Howl } from 'howler';
import Sound from './assets/sound.wav';
import MyButton from './components/MyButton.vue';

const volumeValue = ref(50);
let sound = null;

const changeVolumeValue = (newValue) => {
  if (sound) {
    sound.volume(newValue / 100);
  }
};

const moveSliderLeft = () => {
  volumeValue.value = 0;
};

const moveSliderRight = () => {
  volumeValue.value = 100;
};

onMounted(() => {
  sound = new Howl({
    src: [Sound],
    volume: volumeValue.value / 100
  });

  watch(volumeValue, changeVolumeValue);
});

const playSound = () => {
  sound.play();
};
</script>