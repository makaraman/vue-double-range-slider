<template>
  <div class="w-full relative">
    <input
      type="range"
      :step="step"
      :min="min"
      :max="max"
      :value="minValue"
      class="absolute pointer-events-none appearance-none z-20 h-2 w-full opacity-0"
    />

    <input
      type="range"
      :step="step"
      :min="min"
      :max="max"
      :value="maxValue"
      class="absolute pointer-events-none appearance-none z-20 h-2 w-full opacity-0"
    />

    <div class="block my-5">
      <div class="flex align-items-center justify-between">
        <div v-if="labels.length">{{ labels[0] }}</div>
        <div v-if="labels.length > 0">{{ labels[1] }}</div>
      </div>
    </div>
    <div class="relative z-10 h-2">
      <!-- Empty Bar -->
      <div
        class="absolute z-10 left-0 right-0 bottom-0 top-0 rounded-md bg-gray-200"
      ></div>
      <!-- ./Empty Bar -->

      <!-- Bar -->
      <div
        class="absolute z-20 top-0 bottom-0 rounded-md bg-red-300"
        :style="{ right: `${maxPos}%`, left: `${minPos}%` }"
      ></div>
      <!-- ./Bar -->

      <!-- Min Value -->
      <div
        class="absolute z-30 w-6 h-6 top-0 left-0 bg-red-300 rounded-full -mt-2 -ml-1 cursor-pointer"
        @mousedown="onLeftThumbMouseDown"
        @touchstart="onLeftThumbMouseDown"
        :style="{ left: `${minPos}%` }"
      ></div>
      <!-- ./Min Value -->

      <!-- Max Value -->
      <div
        class="absolute z-30 w-6 h-6 top-0 right-0 bg-red-300 rounded-full -mt-2 -mr-3 cursor-pointer"
        @mousedown="onRightThumbMouseDown"
        @touchstart="onRightThumbMouseDown"
        :style="{ right: `${maxPos}%` }"
      ></div>
      <!-- ./Max Value -->
    </div>
    <div class="block my-5">
      <div class="flex align-items-center justify-between">
        <div>{{ minValue }}</div>
        <div>{{ maxValue }}</div>
      </div>
    </div>
  </div>
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
    modelValue: {
      type: Array,
      default: () => [0, 1],
    },
  },
  data() {
    return {
      minValue: this.modelValue && this.modelValue[0],
      maxValue: this.modelValue && this.modelValue[1],
      maxPos: 0,
      minPos: 0,
      interVal: null,
      startX: null,
      barBox: null,
      barValue: 0,
      thumbMargin: null,
    };
  },
  methods: {
    updateModel() {
      this.$emit("update:modelValue", [this.minValue, this.maxValue]);
    },
    setMinPos() {
      this.minPos = ((this.minValue - this.min) / (this.max - this.min)) * 100;
    },
    setMaxPos() {
      this.maxPos =
        100 - ((this.maxValue - this.min) / (this.max - this.min)) * 100;
    },
    triggerMouseDown(data) {
      const e = data.event;
      const mousemove = data.mousemove;
      const mouseup = data.mouseup;
      const value = data.value;

      e.preventDefault();
      this.startX = e.clientX;

      // Touch Event
      if (e.type === "touchstart") {
        if (e.touches.length === 1) {
          this.startX = e.touches[0].clientX;
        } else {
          return;
        }
      }

      // Current Slider Bar Value
      this.barValue = value;

      // Slider Bar Outer Box
      this.barBox = e.target.parentNode.getBoundingClientRect();

      // Trigger Mousemove event of the left thumb
      document.addEventListener("mousemove", mousemove);
      document.addEventListener("touchmove", mouseup);

      // Block infinity mouse events of the left thumb
      document.addEventListener("mouseup", mouseup);
      document.addEventListener("touchend", mouseup);
    },
    onLeftThumbMouseDown(e) {
      this.triggerMouseDown({
        event: e,
        mousemove: this.onLeftThumbMousemove,
        mouseup: this.onLeftThumbMouseup,
        value: this.minValue,
      });
    },
    onLeftThumbMousemove(e) {
      let clientX = e.clientX;
      if (e.type === "touchmove") {
        clientX = e.touches[0].clientX;
      }

      // The distance of the Left Thumb from the origin
      const distanceOrigin = clientX - this.startX;

      const per = distanceOrigin / this.barBox.width;
      let leftValue = this.barValue + (this.max - this.min) * per;
      let mod = leftValue % this.step;
      leftValue -= mod;

      if (leftValue < this.min) {
        // Range between thumbs
        leftValue = this.min;
      } else if (leftValue > this.maxValue - this.rangeMargin) {
        leftValue = this.maxValue - this.rangeMargin;
      }

      this.minValue = parseFloat(leftValue);
    },
    onLeftThumbMouseup() {
      document.removeEventListener("mousemove", this.onLeftThumbMousemove);
      document.removeEventListener("mouseup", this.onLeftThumbMouseup);
      document.removeEventListener("touchmove", this.onLeftThumbMousemove);
      document.removeEventListener("touchend", this.onLeftThumbMouseup);
    },
    onRightThumbMouseDown(e) {
      this.triggerMouseDown({
        event: e,
        mousemove: this.onRightThumbMousemove,
        mouseup: this.onRightThumbMouseup,
        value: this.maxValue,
      });
    },
    onRightThumbMousemove(e) {
      let clientX = e.clientX;
      if (e.type === "touchmove") {
        clientX = e.touches[0].clientX;
      }

      // The distance of the Left Thumb from the origin
      const distanceOrigin = clientX - this.startX;

      const per = distanceOrigin / this.barBox.width;
      let rightValue = this.barValue + (this.max - this.min) * per;
      let mod = rightValue % this.step;
      rightValue -= mod;

      if (rightValue < this.minValue + this.rangeMargin) {
        // Range between thumbs
        rightValue = this.minValue + this.rangeMargin;
      } else if (rightValue > this.max) {
        rightValue = this.max;
      }

      this.maxValue = parseFloat(rightValue);
    },
    onRightThumbMouseup() {
      document.removeEventListener("mousemove", this.onRightThumbMousemove);
      document.removeEventListener("mouseup", this.onRightThumbMouseup);
      document.removeEventListener("touchmove", this.onRightThumbMousemove);
      document.removeEventListener("touchend", this.onRightThumbMouseup);
    },
  },
  computed: {
    rangeMargin() {
      return parseFloat(
        this.thumbMargin !== null ? this.thumbMargin : this.step
      );
    },
  },
  watch: {
    minValue(newValue) {
      this.setMinPos();
      this.$emit("change:minValue", newValue);
      this.updateModel();
    },
    maxValue(newValue) {
      this.setMaxPos();
      this.$emit("change:minValue", newValue);
      this.updateModel();
    },
  },
};
</script>
