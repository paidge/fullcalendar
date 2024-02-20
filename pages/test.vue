<script setup></script>

<template>
  <div class="parent">
    <Event :x="0" :y="0" :w="64" color="red">
      <p>Event 1</p>
    </Event>
    <Event :x="128" :y="112" :w="128" color="blue">
      <p>Event 2</p>
    </Event>
  </div>
</template>

<style>
@import "vue-draggable-resizable/style.css";

.parent {
  --caseWidth: 64px;
  --caseHeight: 112px;
  height: 1008px;
  width: 960px;
  border: 1px solid red;
  position: relative;
  background: linear-gradient(-90deg, rgba(0, 0, 0, 0.1) 1px, transparent 1px)
      0% 0% / var(--caseWidth) var(--caseHeight),
    linear-gradient(rgba(0, 0, 0, 0.1) 1px, transparent 1px) 0% 0% /
      var(--caseWidth) var(--caseHeight);
}
</style>
