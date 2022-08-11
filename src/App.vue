<template>
  <div class="flex h-screen justify-center items-center">
    <div
      class="p-6 max-w-lg bg-white rounded-lg border-2 border-gray-200 shadow-md"
    >
      <!-- Title -->
      <div class="block mb-3">
        <!-- Component Name -->
        <h5 class="text-2xl font-semibold tracking-tight text-gray-900">
          Vue Range Slider Component
        </h5>
        <!-- ./Component Name -->

        <!-- Author -->
        <small
          >by
          <a href="https://github.com/makaraman" target="_blank"
            >makaraman</a
          ></small
        >
        <!-- ./Author -->
      </div>
      <!-- Title -->

      <!-- RangeSlider -->
      <RangeSlider :min="min" :max="max" v-model="values" />
      <!-- ./RangeSlider -->
    </div>
  </div>
</template>

<script>
import RangeSlider from "./components/RangeSlider/Index.vue";

export default {
  name: "App",
  components: {
    RangeSlider,
  },
  data() {
    return {
      min: 1,
      max: 300,
      values: [50, 295],
    };
  },
};
</script>
