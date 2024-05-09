<template>
    <div>
      <label for="showCompleted">Tampilkan yang belum selesai</label>
      <input type="checkbox" id="showCompleted" v-model="showCompleted" @change="toggleFilter">
    </div>
  </template>
  
  <script>
  import { defineComponent } from 'vue';
  
  export default defineComponent({
    props: ['value'],
    emits: ['update:modelValue', 'toggle-filter'],
    setup(props, { emit }) {
      const showCompleted = props.value;
  
      function toggleFilter() {
        emit('update:modelValue', !showCompleted);
        emit('toggle-filter');
      }
  
      return {
        showCompleted,
        toggleFilter
      };
    }
  });
  </script>
  