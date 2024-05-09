<template>
    <div>
      <input type="text" v-model="newTask" placeholder="Tambahkan kegiatan baru">
      <button @click="addTask">Tambah</button>
    </div>
  </template>
  
  <script>
  import { ref } from 'vue';
  
  export default {
    setup(props, { emit }) {
      const newTask = ref('');
  
      function addTask() {
        if (newTask.value.trim() !== '') {
          const task = {
            id: Date.now(),
            title: newTask.value,
            completed: false
          };
          emit('task-added', task);
          newTask.value = '';
        }
      }
  
      return {
        newTask,
        addTask
      };
    }
  };
  </script>

  <style scoped>
  button {
    background-color: green;
    border-color: transparent;
    margin-left: 5px;
  }

  input{
    background-color: transparent;
    color: white;
  }
</style>
  