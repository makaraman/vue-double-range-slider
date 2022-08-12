<template>
  <div class="flex h-screen justify-center items-center">
    <div
      class="p-6 max-w-lg bg-white rounded-lg border-2 border-gray-200 shadow-md"
    >
      <!-- Title -->
      <div class="block mb-3">
        <!-- Component Name -->
        <h5 class="text-2xl font-semibold tracking-tight text-gray-900">
          Vue Double Range Slider Component
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

      <hr class="my-3" />
      <!-- RangeSlider -->
      <RangeSlider step="10" min="1" max="100" v-model="values" />
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
      values: [1, 100],
    };
  },
};
</script>
