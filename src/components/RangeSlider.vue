<template>
  <div class="slider">
    <slot name="left"></slot>

    <div class="slider__container"
         @mousedown="startDrag"
         @touchstart="startDrag"
         @mousemove="moveHandle"
         @touchmove="moveHandle"
         @mouseup="endDrag"
         @touchend="endDrag"
         ref="sliderContainer">
      <div class="slider__track" :style="trackStyle" ref="track"></div>
      <div class="slider__handle"
           tabindex="0"
           :style="{ left: volume + '%' }"
           ref="handle"
           @keydown="handleKeyDown"></div>
    </div>

    <slot name="right"></slot>
  </div>
</template>

<script>
export default {
  props: {
    modelValue: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      volume: this.modelValue,
      dragging: false,
      animating: false,
      sliderWidth: 0,
      sliderRect: null,
    };
  },
  computed: {
    trackStyle() {
      return {
        background: `linear-gradient(to right, var(--colorPrimary) ${this.volume}%, #ce9584 ${this.volume}%)`,
      };
    },
  },
  watch: {
    modelValue(newValue) {
      this.volume = newValue;
    },
  },
  mounted() {
    this.updateSliderDimensions();
    window.addEventListener('resize', this.updateSliderDimensions);
    window.addEventListener('mousemove', this.moveHandle);
    window.addEventListener('touchmove', this.moveHandle);
    window.addEventListener('mouseup', this.stopDrag);
    window.addEventListener('touchend', this.stopDrag);
  },
  beforeUnmount() {
    window.removeEventListener('resize', this.updateSliderDimensions);
    window.removeEventListener('mousemove', this.moveHandle);
    window.removeEventListener('touchmove', this.moveHandle);
    window.removeEventListener('mouseup', this.stopDrag);
    window.removeEventListener('touchend', this.stopDrag);
  },
  methods: {
    updateSliderDimensions() {
      this.sliderWidth = this.$refs.sliderContainer.clientWidth;
      this.sliderRect = this.$refs.sliderContainer.getBoundingClientRect();
    },
    startDrag(event) {
      event.stopPropagation();
      this.dragging = true;
      this.moveHandle(event);
    },
    moveHandle(event) {
      if (this.dragging) {
        const clientX = event.type === 'touchstart' ? event.touches[0].clientX : event.clientX;
        const newPosition = (clientX - this.sliderRect.left) / this.sliderWidth * 100;
        const newVolume = Math.max(0, Math.min(100, Math.round(newPosition)));

        if (!this.animating) {
          this.animating = true;
          requestAnimationFrame(() => {
            this.volume = newVolume;
            this.$emit('changeVolumeValue', this.volume);
            this.animating = false;
          });
        }
      }
    },
    stopDrag() {
      if (this.dragging) {
        this.dragging = false;
      }
    },
    handleKeyDown(event) {
      if (event.key === 'ArrowLeft' || event.key === 'ArrowRight') {
        event.preventDefault(); // Prevent scrolling the page with arrow keys
        const step = 1; // Adjust the step value as needed
        let increment = 0;

        if (event.key === 'ArrowLeft') {
          increment = -step;
        } else if (event.key === 'ArrowRight') {
          increment = step;
        }

        const newVolume = Math.max(0, Math.min(100, this.volume + increment));
        this.volume = newVolume;

        this.$emit('changeVolumeValue', this.volume);
      }
    },
  },
};
</script>

<style scoped>
.slider {
  display: flex;
  gap: 10px;
  align-items: center;
  justify-content: center;
  width: 80%;
  margin: 20px auto;
  text-align: center;
}

.slider__container {
  position: relative;
  width: 100%;
  height: 10px;
  background-color: #ddd;
  border-radius: 5px;
  cursor: pointer;
}

.slider__track {
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  border-radius: 5px;
  width: 100%;
}

.slider__handle {
  position: absolute;
  top: -5px;
  width: 20px;
  height: 20px;
  background-color: var(--colorPrimary);
  border-radius: 50%;
  transform: translateX(-50%);
  cursor: pointer;
  user-select: none;

  &:hover {
    box-shadow: 0px 0px 0px 4px rgba(255, 110, 64, 0.3);
    transition: all 0.2s ease-out;
  }

  &:focus-within {
    outline: none;
    box-shadow: 0px 0px 0px 4px rgba(255, 110, 64, 0.3);
  }

  &:active {
    box-shadow: 0px 0px 0px 4px rgba(255, 110, 64, 0.6);
  }
}

</style>