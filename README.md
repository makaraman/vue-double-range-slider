
# Vue Double Range Slider Component
A Component created with VueJs v3 & TailwindCSS

## Check it out
[Live Demo](https://vue-double-range-slider.vercel.app/)

## Props

| Name | Type   | Default     | Description                |
| :--- | :----- | :---------- | :--------------------------|
| `min` | String \| Number | 0 | Minimum value of the range slider |
| `max` | String \| Number | 0 | Maximum value of the range slider |
| `step` | String \| Number | 0 | It is used to specify how many digits to jump at once|
| `labels` | Array | ['Min.', 'Max.'] | Labels for the min & max value|
| `modelValue` | Array | [0, 1] | Key for v-model. It emit events with [min, max] values|

## Events  

| Name | Payload | Description                |
| :--- | :------ | :--------------------------|
| `update:modelValue` | Array [min, max] | Event for v-model key. Similar with `$emit('input', value)` |
| `change:minValue` | Number | Min. Value |
| `change:maxValue` | Number | Max. Value |
