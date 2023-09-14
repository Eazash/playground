<script setup lang="ts">
  const rangeInputRef = ref<HTMLInputElement | null>(null)
  const min = ref(1)
  const max = ref(10)
  const delta = computed(() => max.value - min.value)

  const labels = computed(() => {
    return Array.from({ length: delta.value + 1 }, (_, index) => index + min.value)
  })

  function updateSliderBackground() {
    if (rangeInputRef.value !== null) {
      const sliderPosition = ((+rangeInputRef.value.value - min.value) / delta.value) * 100
      console.log(sliderPosition)
      rangeInputRef.value.style.background = `linear-gradient(to right, #f50 ${sliderPosition}%, #ccc ${sliderPosition}%)`
    }
  }
  onMounted(() => {
    updateSliderBackground()
  })
</script>
<template>
  <div class="range">
    <input ref="rangeInputRef" @input="updateSliderBackground" class="range-input" type="range" name="rating" step="1"
      :min="min" :max="max" list="range-values">
    <ul class="label-wrapper">
      <li class="range-label" v-for="value in labels" :key="value">{{ value }}</li>
    </ul>
  </div>
</template>
<style>
  :root {
    --range-height: 15px;
    --thumb-border-width: 2px;
    --thumb-diameter: calc(var(--range-height) * 1.75);
    --thumb-radius: calc(var(--thumb-diameter) / 2);
  }

  .range {
    max-width: 50vw;
    margin: 1rem auto 0.5rem;

    @media (max-width: 520px) {
      max-width: 90vw;
    }
  }

  .range-input {
    -webkit-appearance: none;
    appearance: none;
    width: 100%;
    cursor: pointer;
    outline: none;

    background: #ccc;
    height: var(--range-height);
    border-radius: var(--range-height);
  }

  .range-input::-webkit-slider-thumb {
    appearance: none;
    height: var(--thumb-diameter);
    width: var(--thumb-diameter);
    background-color: #fff;
    border: 2px solid green;
    border-radius: var(--thumb-radius);
    transition: .2s cubic-bezier(0.06, 1.68, 1, 1.38);
  }

  .range-input::-webkit-slider-thumb:hover {
    box-shadow: 0 0 0 calc(var(--thumb-radius) / 3) hsla(20, 100%, 50%, 0.25);
  }

  .range-input:active::-webkit-slider-thumb {
    box-shadow: 0 0 0 calc(var(--thumb-radius) / 2) hsla(20, 100%, 50%, 0.5);
  }

  .range-input:focus::-webkit-slider-thumb {
    box-shadow: 0 0 0 calc(var(--thumb-radius) / 2) hsla(20, 100%, 50%, 0.5);
  }

  .range-input::-moz-range-thumb {
    appearance: none;
    height: calc(var(--thumb-diameter) - 4px);
    width: calc(var(--thumb-diameter) - 4px);
    background-color: #fff;
    border: 2px solid green;
    border-radius: var(--thumb-radius);
    transition: .2s cubic-bezier(0.06, 1.68, 1, 1.38);
  }


  .range-input::-moz-range-thumb:hover {
    box-shadow: 0 0 0 calc(var(--thumb-radius) / 3) hsla(20, 100%, 50%, 0.25);
  }

  .range-input:active::-moz-range-thumb {
    box-shadow: 0 0 0 calc(var(--thumb-radius) / 2) hsla(20, 100%, 50%, 0.5);
  }

  .range-input:focus::-moz-range-thumb {
    box-shadow: 0 0 0 calc(var(--thumb-radius) / 2) hsla(20, 100%, 50%, 0.5);
  }
</style>