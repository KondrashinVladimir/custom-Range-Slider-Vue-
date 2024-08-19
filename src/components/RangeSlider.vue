<template>
  <div class="slider">
    <slot name="left"></slot>
    <div class="slider__container">
      <input
        type="range"
        class="range-slider"
        min="0"
        max="100"
        v-model="volumeValue"
        :style="{ background: changeBackgroundColor }"
      />
    </div>
    <slot name="right"></slot>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue';

const props = defineProps({
  modelValue: {
    type: Number,
    required: true
  }
});

const emit = defineEmits(['update:modelValue']);

const volumeValue = ref(props.modelValue);

const changeBackgroundColor = computed(() => {
  const value = (volumeValue.value - 0) / (100 - 0) * 100;
  return `linear-gradient(to right, rgba(255, 110, 64, 1) ${value}%, rgba(255, 110, 64, 0.4) ${value}%)`;
});

watch(volumeValue, (newValue) => {
  emit('update:modelValue', newValue);
});

watch(() => props.modelValue, (newValue) => {
  volumeValue.value = newValue;
});
</script>


<style lang="scss">
.slider {
  display: flex;
  flex-grow: 1;
  flex-shrink: 1;
  flex-basis: auto;
  gap: 10px;
  align-items: center;
  justify-content: space-between;
  width: 304px;
  margin: 20px auto;
  text-align: center;
}

.slider__container {
  display: flex;
  width: 200px;
  
}

.range-slider {
  -webkit-appearance: none;
  width: 100%;
  height: 8px;
  border: none;
  border-radius: 5px;
  outline: none;
  padding: 0;
  margin: 0;
}

/* Для Chrome, Safari, Opera */
.range-slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: var(--colorPrimary);
  cursor: pointer;
}

.range-slider:focus-visible::-webkit-slider-thumb,
.range-slider:hover::-webkit-slider-thumb {
  box-shadow: 0px 0px 0px 8px rgba(255, 110, 64, 0.3); 
}

/* Для Firefox */
.range-slider::-moz-range-thumb {
  width: 20px;
  height: 20px;
  border: none;
  border-radius: 50%;
  background: var(--colorPrimary);
  cursor: pointer;
}

.range-slider:focus-visible::-moz-range-thumb,
.range-slider:hover::-moz-range-thumb {
  box-shadow: 0px 0px 0px 8px rgba(255, 110, 64, 0.3); 
}

/* Для Internet Explorer и старых версий Edge */
.range-slider::-ms-thumb {
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: var(--colorPrimary);
  cursor: pointer;
}

.range-slider:focus-visible::-ms-thumb,
.range-slider:hover::-ms-thumb {
  box-shadow: 0px 0px 0px 8px rgba(255, 110, 64, 0.3); 
}
</style>