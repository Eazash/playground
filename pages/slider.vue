<template>
  <div class="range"
    :style="{ '--thumb-radius': thumbRadius + 'px', '--background-width': thumbRadius + sliderPosition + 'px' }">
    <input v-model="data" ref="rangeInputEl" class="range-input" type="range" name="rating" :min="min" :max="max">
    <div class="value-indicator" :class="{ 'visible': inputPressed, 'value-indicator-top': indicatorPosition === 'top', 'value-indicator-bottom': indicatorPosition=== 'bottom'}" ref="valueIndicatorRef"><span
        class="value-indicator-span">{{ data }}</span></div>
  </div>
</template>
<script setup lang="ts">
  const valueIndicatorRef = ref<HTMLSpanElement | null>(null)
  const rangeInputEl = ref<HTMLInputElement | null>(null)
  const min = ref(0)
  const max = ref(120)
  const thumbRadius = ref(16);
  const data = ref(9)
  const indicatorPosition = ref<"bottom"|"top">("top")
  const { width: inputWidth } = useElementSize(rangeInputEl)
  const { pressed: inputPressed } = useMousePressed({target: rangeInputEl.value})
  const delta = computed(() => max.value - min.value)

  const sliderPosition = computed(() => {
    const percentagePosition = (+data.value - min.value) / delta.value;
    const pixelPosition = inputWidth.value * percentagePosition
    const centerPosition = inputWidth.value / 2;
    const percentageDistanceToCenter = (pixelPosition - centerPosition) / centerPosition;
    const offset = (thumbRadius.value) * percentageDistanceToCenter

    return pixelPosition - thumbRadius.value - offset;
  })
  watchSyncEffect(() => {
    if (valueIndicatorRef.value !== null) {
      valueIndicatorRef.value.style.left = sliderPosition.value + thumbRadius.value + "px";
    }
  })
</script>


<style>
  * {
    box-sizing: border-box;
  }

  .range {
    --thumb-border-width: 2px;
    --thumb-diameter: calc(var(--thumb-radius)* 2);
    --range-height: calc(var(--thumb-diameter) / 1.75);

    max-width: 50vw;
    margin: 2rem auto calc(var(--thumb-radius) / 2);
    padding-block: 6px;
    position: relative;
  }

  @media (max-width: 520px) {
    .range {
      max-width: 90vw;
    }
  }

  .range-input {
    appearance: none;
    width: 100%;
    cursor: pointer;
    outline: none;

    background: linear-gradient(to right, #f50 var(--background-width), #ccc var(--background-width));
    height: var(--range-height);
    border-radius: var(--range-height);
    transition: all 0.2s;
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

  .range-input:active::-webkit-slider-thumb {
    box-shadow: 0 0 0 2px hsla(20, 100%, 50%, 0.25);
  }

  /* .range-input:active::-webkit-slider-thumb {
      box-shadow: 0 0 0 calc(var(--thumb-radius) / 2) hsla(20, 100%, 50%, 0.5);
    }

    .range-input:focus::-webkit-slider-thumb {
      box-shadow: 0 0 0 calc(var(--thumb-radius) / 2) hsla(20, 100%, 50%, 0.5);
    } */

  .range-input::-moz-range-thumb {
    appearance: none;
    height: calc(var(--thumb-diameter) - 4px);
    width: calc(var(--thumb-diameter) - 4px);
    background-color: #fff;
    border: 2px solid green;
    border-radius: var(--thumb-radius);
    transition: .2s cubic-bezier(0.06, 1.68, 1, 1.38);
  }


  .range-input:active::-moz-range-thumb:hover {
    box-shadow: 0 0 0 2px hsla(20, 100%, 50%, 0.25);
  }

  /* .range-input:active::-moz-range-thumb {
      box-shadow: 0 0 0 calc(var(--thumb-radius) / 2) hsla(20, 100%, 50%, 0.5);
    }

    .range-input:focus::-moz-range-thumb {
      box-shadow: 0 0 0 calc(var(--thumb-radius) / 2) hsla(20, 100%, 50%, 0.5);
    } */

  .value-indicator {
    display: inline-flex;
    z-index: -1;
    justify-content: center;
    min-width: var(--thumb-diameter);
    height: calc(var(--thumb-diameter) * 1.65);
    padding: calc(var(--thumb-radius) / 4);
    border-radius: var(--thumb-radius);

    position: absolute;
    transform: translate(-44%, 0%);

    color: #fff;
    background-color: #f50;

    font-size: var(--thumb-radius);
    line-height: var(--thumb-radius);


    visibility: hidden;
    opacity: 0;
    transition: visibility 0.1s, opacity 0.2s linear, position 0.2s linear;
  }

  .value-indicator-bottom{ 
    align-items: flex-end;
    top: 1px;
  }
  .value-indicator-top{
    align-items: flex-start;
    bottom: 4px;
  }

  .visible {
    visibility: visible;
    opacity: 1;
  }
</style>