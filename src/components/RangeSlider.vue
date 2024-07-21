<template>
  <div class="slider" @touchstart.prevent="startDrag">
    <slot name="left"></slot>

    <div class="slider__container" @mousedown.prevent="startDrag" ref="sliderContainer">
      <div class="slider__track" :style="widthStyle"></div>
      <div class="slider__handle" tabindex="0" :style="{ left: volumeValue + '%' }" @keydown="handleKeyDown">
      </div>
    </div>

    <slot name="right"></slot>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount, computed } from 'vue';

const volumeValue = defineModel();
const dragging = ref(false);
const animating = ref(false);
const sliderWidth = ref(null);
const sliderRect = ref(null);
const sliderContainer = ref(null);
const emit = defineEmits(['update:modelValue']);

onMounted(() => {
  updateSliderDimensions();
  window.addEventListener('resize', updateSliderDimensions);
});

onBeforeUnmount(() => {
  window.removeEventListener('resize', updateSliderDimensions);
});

const updateSliderDimensions = () => {
  sliderWidth.value = sliderContainer.value.clientWidth;
  sliderRect.value = sliderContainer.value.getBoundingClientRect();
};

const widthStyle = computed(() => {
  return {
    width: `${volumeValue.value}%`,
  };
});

const startDrag = (event) => {
  dragging.value = true;
  if (event.type === 'mousedown') {
    document.addEventListener('mousemove', moveHandle);
    document.addEventListener('mouseup', stopDrag);
  } else if (event.type === 'touchstart') {
    document.addEventListener('touchmove', moveHandle);
    document.addEventListener('touchend', stopDrag);
  }
  moveHandle(event);
};

const stopDrag = () => {
  dragging.value = false;
  document.removeEventListener('mousemove', moveHandle);
  document.removeEventListener('mouseup', stopDrag);
  document.removeEventListener('touchmove', moveHandle);
  document.removeEventListener('touchend', stopDrag);
};

const moveHandle = (event) => {
  if (dragging.value) {
    let clientX = null;
    if (event.type === 'mousedown' || event.type === 'mousemove') {
      clientX = event.clientX;
    } else if (event.type === 'touchstart' || event.type === 'touchmove') {
      clientX = event.touches[0].clientX;
    }

    const newPosition = (clientX - sliderRect.value.left) / sliderWidth.value * 100;
    const newVolume = Math.max(0, Math.min(100, newPosition));

    if (!animating.value) {
      animating.value = true;
      requestAnimationFrame(() => {
        volumeValue.value = Math.round(newVolume);
        emit('update:modelValue', Math.round(newVolume))
        animating.value = false;
      });
    }
  }
};

const handleKeyDown = (event) => {
  if (event.key === 'ArrowLeft' || event.key === 'ArrowRight') {
    event.preventDefault();
    const step = 1;
    let increment = 0;

    if (event.key === 'ArrowLeft') {
      increment = -step;
    } else if (event.key === 'ArrowRight') {
      increment = step;
    }

    const newVolume = Math.max(0, Math.min(100, volumeValue.value + increment));
    
    volumeValue.value = Math.round(newVolume);
    emit('update:modelValue', Math.round(newVolume))
  }
};
</script>

<style scoped>
.slider {
  display: flex;
  flex-grow: 1;
  gap: 10px;
  align-items: center;
  justify-content: space-between;
  width: 304px;
  margin: 20px auto;
  text-align: center;
}

.slider__container {
  position: relative;
  width: 176px;
  height: 8px;
  background-color: rgba(255, 110, 64, 0.4);
  border-radius: 5px;
  cursor: pointer;
}

.slider__track {
  position: absolute;
  background-color: var(--colorPrimary);
  top: 0;
  left: 0;
  height: 100%;
  border-radius: 5px;
  transition: all 0.1s ease-out;
}

.slider__handle {
  position: absolute;
  top: -6px;
  width: 20px;
  height: 20px;
  background-color: var(--colorPrimary);
  border-radius: 50%;
  transform: translateX(-50%);
  cursor: pointer;
  user-select: none;
  transition: all 0.1s ease-out;

  &:hover {
    box-shadow: 0px 0px 0px 6px rgba(255, 110, 64, 0.3);
    transition: all 0.1s ease-out;
  }

  &:focus-within {
    outline: none;
    box-shadow: 0px 0px 0px 6px rgba(255, 110, 64, 0.3);
  }

  &:active {
    box-shadow: 0px 0px 0px 6px rgba(255, 110, 64, 0.5);

  }
}
</style>