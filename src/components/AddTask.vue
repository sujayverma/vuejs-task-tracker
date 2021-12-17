<template>
    <form @submit="onSubmit" class="add-form">
        <div class="form-control">
            <label>Task</label>
            <input type="text" name="text" v-model="text" placeholder="Add Task" />
        </div>
        <div class="form-control">
            <label>Day & Time</label>
            <input type="text" name="day" v-model="day" placeholder="Day & Time" />
        </div>
        <div class="form-control form-control-check">
            <label>Set Reminder</label>
            <input type="checkbox" name="reminder" v-model="reminder" />
        </div>
        <div v-if="editFormShow">
          <input type="button" value="Update task" class="btn btn-block" @click="updateData" />
        </div>
        <div v-else >
        <input type="submit" value="Save Task" class="btn btn-block" />
        </div>
    </form>
</template>

<script>
export default {
  name: 'AddTask',
  props: {
    edittask: Object,
    editFormShow: Boolean
  },
  data () {
    return {
      id: '',
      text: '',
      day: '',
      reminder: false
    }
  },
  methods: {
    onSubmit (e) {
      e.preventDefault()
      if (!this.text) {
        alert('Please add task!!')
        return
      }
      const newTask = {
        text: this.text,
        day: this.day,
        reminder: this.reminder
      }
      this.$emit('add-task', newTask)
      this.text = ''
      this.day = ''
      this.reminder = false
    },
    updateData () {
      const updateTask = {
        id: this.id,
        text: this.text,
        day: this.day,
        reminder: this.reminder
      }
      this.$emit('update-task', updateTask)
      this.id = ''
      this.text = ''
      this.day = ''
      this.reminder = false
    }
  },
  emits: ['add-task', 'update-task'],
  watch: {
    $props: {
      handler: function (val, oldVal) {
        if (this.editFormShow) {
          this.text = this.edittask.text
          this.day = this.edittask.day
          this.reminder = this.edittask.reminder
          this.id = this.edittask.id
        } else {
          this.text = ''
          this.day = ''
          this.reminder = false
          this.id = ''
        }
      },
      deep: true
    }
  }

}
</script>

<style scoped>
.add-form {
  margin-bottom: 40px;
}
.form-control {
  margin: 20px 0;
}
.form-control label {
  display: block;
}
.form-control input {
  width: 100%;
  height: 40px;
  margin: 5px;
  padding: 3px 7px;
  font-size: 17px;
}
.form-control-check {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.form-control-check label {
  flex: 1;
}
.form-control-check input {
  flex: 2;
  height: 20px;
}
</style>
