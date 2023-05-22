<script setup>
import { computed } from "vue";

const props = defineProps({
  width: {
    type: Number,
    default: 0,
  },
  height: {
    type: Number,
    default: 0,
  },
  left: {
    type: Number,
    default: 0,
  },
  top: {
    type: Number,
    default: 0,
  }
});

const actWidth = computed(() =>props.width);
const actHeight = computed(() => props.height);
const actLeft = computed(() => props.left);
const actTop = computed(() => props.top);
</script>

<template>
  <div
    class="widgetBox"
    :style="{
      width: actWidth + 'px',
      height: actHeight + 'px',
      left: actLeft + 'px',
      top: actTop + 'px',
    }"
  >
    <slot></slot>
  </div>
</template>

<style scoped>
.widgetBox {
  position: absolute;
  overflow: hidden;
}
</style>
