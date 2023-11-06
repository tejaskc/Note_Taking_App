<script setup lang="ts">
import { defineProps, defineEmits } from "vue";
const props = defineProps(["note"]);
const emitNote = defineEmits(["editNote", "deleteNote"]);
</script>

<template>
  <div class="p-1">
    <div style="display: flex">
      <i
        class="fa-regular fa-pen-to-square"
        @click="emitNote('editNote', props.note.id)"
        style="align-items: center; cursor: pointer"
      ></i
      >&nbsp;&nbsp;
      <i
        class="fas fa-trash-alt"
        @click="emitNote('deleteNote', props.note.id)"
        style="cursor: pointer"
      ></i>
    </div>
    <p class="main-text">{{ props.note.text }}</p>
    <p class="date">{{ props.note.date.toLocaleDateString("en-US") }}</p>
  </div>
</template>
<style scoped>
.date {
  font-size: 12.5px;
  font-weight: bold;
}
</style>
