<template>
  <div v-if="visible" class="face-card">
    {{ props.name }}
    <br>
    <span class="value-container"> {{ props.value }} </span>
  </div>
  <div v-else class="back-card">
    ❤️
  </div>
</template>

<script setup>
import { defineProps } from "vue";
const props = defineProps(["name", "value", "visible", "index"]);
</script>

<style scoped>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
.face-card, .back-card {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 50px;
  padding: 30px;
  border: 1px solid black;
  border-radius: 10px;
  margin: 10px;
}
.value-container{
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
