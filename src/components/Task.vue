<template>
    <div @dblclick="$emit('toggle-reminder', task.id)" :class="[task.reminder ? 'reminder' : '', 'task']">
        <h3>{{task.text}}
          <div>
            <i @click="onEdit(task)" class="fas-pen fas fa-edit"></i>
            <i @click="onDelete(task.id)" class="fas fa-times"></i>
          </div>
        </h3>
        <p> {{task.day}}</p>
    </div>
</template>

<script>
export default {
  name: 'Task',
  props: {
    task: Object
  },
  methods: {
    onDelete (id) {
      this.$emit('delete-task', id)
    },
    onEdit (task) {
      this.$emit('edit-task', task)
    }
  }
}
</script>

<style scoped>
.fas {
    color: red;
}
.fas-pen {
  color: green;
  margin-right: 5px;
}
.task {
    background: #f4f4f4;
    margin: 5px;
    padding: 10px 20px;
    cursor: pointer;
}

.task.reminder {
    border-left: 5px solid green;
}

.task h3 {
    display: flex;
    align-items: center;
    justify-content: space-between;
}
</style>
