<template>
  <div class="container" style="max-width: 600px">
    <!-- heading start  -->
    <h2 class="text-center mt-5">My Vue To-Do App</h2>

    <form class="d-flex mt-5" @submit.prevent="addTask">
      <input
        type="text"
        v-model="task"
        placeholder="Enter Task"
        class="w-100 form-control"
      />
      <button @click="submitTask" class="btn btn-warning rounded-0">SUBMIT</button>
    </form>

    <!-- Task Table  -->
    <table class="table table-bordered mt-4">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col" style="width: 120px;">Status</th>
          <th scope="col" class="text-center">Edit</th>
          <th scope="col" class="text-center">Delete</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span :class="{'done':task.status === 'done'}">
              {{ task.name }}
            </span>
          </td>
          <td>
            <span @click="changeStatus(index)" class="pointer"
             :class="{'text-danger':task.status === 'to-do',
             'text-success': task.status === 'done' 
            }"
            
             >

              {{ firstCharUpper(task.status) }}
            </span>
          </td>
          <td> 
            <div class="text-center" @click="editTask(index)">
              <span class="fa fa-pencil"></span>
            </div>
          </td>
          <td class="text-center" @click="deleteTask(index)">
            <div>
              <span class="fa fa-trash"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default{
  name: 'TodoApp',
  props: {
    msg:String,
  },
  data(){
    return{
      task:'',
      editedTask: null,
      tasks: JSON.parse(window.localStorage.getItem("tasks")),
      availableStatuses: ['to-do', 'done']
    }
  },
  methods: {
    submitTask(){
      if(this.task.length === 0) return;

      if(this.editedTask === null){
      this.tasks.push({
        name:this.task,
        status: 'to-do'
      })
    } else{
      this.tasks[this.editedTask].name = this.task;
      this.editedTask = null;
       
    }
     window.localStorage.setItem('tasks', JSON.stringify(this.tasks))
    this.task = '';
    },
    
    editTask(index){
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },
    deleteTask(index){
      this.tasks.splice(index, 1);
      window.localStorage.setItem('tasks', JSON.stringify(this.tasks))
    },
    changeStatus(index) {
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      if(++newIndex > 1) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex];
  },
  firstCharUpper(str){
    return str.charAt(0).toUpperCase() + str.slice(1)
  }
}

}
</script>

<style scoped>
.pointer {
  cursor:pointer;
}
.done {
  text-decoration: line-through;
}
</style>

