<template>
    <tr>
      <td :style="{ textDecoration: task.completed ? 'line-through' : 'none', color: 'white' }">{{ task.title }}</td>
      <td :style="{ color: 'white' }">{{ task.completed ? 'Selesai' : 'Belum Selesai' }}</td>
      <td>
        <button @click="toggleStatus" class="btn" :class="{'btn-success': !task.completed, 'btn-warning': task.completed}" :style="{'marginLeft' :'10px'}">
          {{ task.completed ? 'Batal' : 'Selesai' }}
        </button>
        <button v-if="!task.completed" @click="deleteTask" class="btn btn-danger" :style="{'marginLeft' :'10px'}">Hapus</button>
        <button v-if="!task.completed" @click="editTask" class="btn btn-info" :style="{'marginLeft' :'10px'}">Edit</button>
      </td>
    </tr>
  </template>
  
  
  <script>
  import { defineComponent } from 'vue';
  import Swal from 'sweetalert2'; 
  
  export default defineComponent({
    props: ['task'],
    emits: ['cancel-task', 'complete-task', 'delete-task', 'update-task'], 
    methods: {
      toggleStatus() {
        if (this.task.completed) {
          this.$emit('cancel-task', this.task.id);
        } else {
          this.$emit('complete-task', this.task.id);
        }
      },
      deleteTask() {
        this.$emit('delete-task', this.task.id);
      },
      editTask() {
        Swal.fire({
          title: 'Edit Task',
          input: 'text',
          inputValue: this.task.title,
          showCancelButton: true,
          confirmButtonText: 'Save',
          cancelButtonText: 'Cancel',
          inputValidator: (value) => {
            if (!value) {
              return 'You need to write something!';
            }
          }
        }).then((result) => {
          if (result.isConfirmed) {
            this.task.title = result.value;
            this.$emit('update-task', this.task);
          }
        });
      }
    }
  });
  </script>
  