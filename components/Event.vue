<script setup>
import VueDraggableResizable from "vue-draggable-resizable";

const props = defineProps(["x", "y", "w", "color"]);
const event = ref();

const caseWidth = 64;
const caseHeight = 112; // h-28
const eventHeight = 112;

function onDragStop(x, y) {
  const $this = event.value;

  $this.left = arrondi($this.left, caseWidth);
  $this.top = arrondi($this.top, caseHeight);
  $this.height = eventHeight;
  $this.bottom = $this.parentHeight - $this.height - $this.top;
  $this.right = $this.parentWidth - $this.width - $this.left;
}

function onresizeStop() {
  const $this = event.value;
  $this.width = arrondi($this.width, caseWidth);
  $this.right = $this.parentWidth - $this.width - $this.left;
}

// function resetSize() {
//   const $this = event.value;
//   $this.left = props.x;
//   $this.top = props.y;
//   $this.width = props.w;
//   $this.height = eventHeight;
//   $this.bottom = $this.parentHeight - $this.height - $this.top;
//   $this.right = $this.parentWidth - $this.width - $this.left;
// }

function arrondi(val, base) {
  const modulo = val % base;
  const bornes = {
    debut: Math.floor(val / base) * base,
    fin: Math.ceil(val / base) * base,
  };

  return modulo < base / 2 ? bornes.debut : bornes.fin;
}
</script>

<template>
  <vue-draggable-resizable
    ref="event"
    class="event absolute"
    :style="{ backgroundColor: color }"
    :x="x"
    :y="y"
    :w="w"
    :h="eventHeight"
    :min-width="caseWidth"
    :max-width="caseWidth * 4"
    :parent="true"
    :active="true"
    :prevent-deactivation="true"
    :disableUserSelect="true"
    :handles="['mr']"
    @dragStop="onDragStop"
    @resizeStop="onresizeStop"
  >
    <slot></slot>
  </vue-draggable-resizable>
</template>

<style scoped>
.event {
  cursor: move;
  /* transition: all 100ms ease-in-out; */
}

.event.dragging,
.event.resizing {
  opacity: 0.7;
  z-index: 10 !important;
}
</style>
