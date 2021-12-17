<template>
    <div v-for="task of tasks" :key="task.id">
        <Task @edit-task="$emit('edit-task', task)" @toggle-reminder="$emit('toggle-reminder', task.id)" @delete-task="$emit('delete-task', task.id)" :task="task" />
    </div>
</template>

<script>
import Task from './Task.vue'

export default {
  name: 'Tasks',
  props: {
    tasks: Array
  },
  components: {
    Task
  },
  emits: ['delete-task', 'toggle-reminder', 'edit-task']
}
</script>
