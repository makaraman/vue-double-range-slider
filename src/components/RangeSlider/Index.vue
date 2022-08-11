<template>
  <div class="w-full bg-blue-400">here</div>
</template>
<script>
export default {
  name: "RangeSlider",
  props: {
    min: {
      default: 0,
    },
    max: {
      default: 1,
    },
    step: {
      default: 0.1,
    },
    labels: {
      type: Array,
      default: () => ["Min.", "Max."],
    },
    value: {
      type: Array,
      default: () => [0, 100],
    },
  },
  model: {
    prop: "value",
  },
};
</script>
