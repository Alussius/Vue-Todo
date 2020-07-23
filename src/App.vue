<template>
  <div id="app">
    <h4 class="bg-primary text-white text-center p-2">
      {{ name }}'s To Do List
    </h4>
    <div class="container-fluid p-4">
      <div class="row" v-if="filteredTasks.length == 0">
        <div class="col text-center">
          <strong>You have no task to do at the moment.</strong>
        </div>
      </div>
      <template v-else>
        <div class="row">
          <div class="col font-weight-bold">Task</div>
          <div class="col-2 font-weight-bold">Done</div>
        </div>
        <div class="row" v-for="t in filteredTasks" :key="t.action">
          <div class="col">{{ t.action }}</div>
          <div class="col-2 text-center">
            <input type="checkbox" v-model="t.done" class="form-check-input" />
          </div>
        </div>
      </template>
      <div class="row bg-secondary py-2 mt-2 text-white">
        <div class="col text-center">
          <input type="checkbox" v-model="hideCompleted" class="form-check-input" />
          <label class="form-check-label font-weight-bold">Hide Completed Tasks</label>
        </div>
        <div class="col text-center">
          <button class="btn btn-sm btn-warning" @click="deleteCompleted">Delete Completed</button>
        </div>
      </div>
      <div class="row py-2">
        <div class="col">
          <input type="text" v-model="newItemText" class="form-control">
        </div>
        <div class="col-2">
          <button @click="addNewTodo" class="btn btn-primary">Add</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      name: "Rue Dewprism",
      tasks: [],
      hideCompleted: true,
      newItemText: ""
    }
  },
  computed: {
    filteredTasks() {
      return this.hideCompleted ? this.tasks.filter(t => !t.done) : this.tasks
    }
  },
  methods: {
    addNewTodo() {
      this.tasks.push({ action: this.newItemText, done: false });
      this.storeData();
      this.newItemText = "";
    },
    storeData() {
      localStorage.setItem("todos", JSON.stringify(this.tasks));
    },
    deleteCompleted() {
      this.tasks = this.tasks.filter(t => !t.done);
      this.storeData();
    }
  },
  created() {
    let data = localStorage.getItem("todos");
    if (data != null) {
      this.tasks = JSON.parse(data);
    }
  }
}
</script>
