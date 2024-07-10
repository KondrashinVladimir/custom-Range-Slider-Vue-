<template>
  <div>
    <div>
      <button @click="playSound">Воспроизвести звук</button>
    </div>
    <RangeSlider @changeVolumeValue="changeVolumeValue" v-model="volumeValue">
      <template v-slot:right>
        <MyButton class="my-button" @click="moveSliderRight">
          <MyIcon class="my-icon my-icon--volume-on"></MyIcon>
        </MyButton>
      </template>
      <template v-slot:left>
        <MyButton class="my-button" @click="moveSliderLeft">
          <MyIcon class="my-icon my-icon--volume-off"></MyIcon>
        </MyButton>
      </template>
    </RangeSlider>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import RangeSlider from './components/RangeSlider.vue';
import { Howl } from 'howler';
import Sound from './assets/sound.wav';
import MyButton from './components/MyButton.vue';
import MyIcon from './components/MyIcon.vue';

const volumeValue = ref(50);
let sound = null;

const changeVolumeValue = (volume) => {
  volumeValue.value = volume;
  if (sound) {
    sound.volume(volumeValue.value / 100);
  }
};

const moveSliderRight = () => {
  volumeValue.value = 100;
  if (sound) {
    sound.volume(1); 
  }
};

const moveSliderLeft = () => {
  volumeValue.value = 0;
  if (sound) {
    sound.volume(0); 
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
