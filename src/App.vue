<script>
import Todos from './components/list.vue'
export default {
  components:{Todos},
  data() {
    return {
      todo:'',
      todos:[]
    }
  },
  mounted() {
    const storage = JSON.parse(localStorage.getItem('todos'))
    if(storage)
      this.todos  = storage
  },
  methods: {
    add(){
       this.todos.unshift({
         activity:this.todo,
         isDone: false
       });

       this.todo = '';

       this.saveToStorage();
    },
    deleteTodo(indexTodo){
     this.todos = this.todos.filter((item,index) => {
        if (index != indexTodo) {
          return item
        }
      });

      this.saveToStorage();
    },
     doneTodo (indexTodo){
      this.todos = this.todos.filter((item,index) => {
        if (index == indexTodo && item.isDone == false) {
          return item.isDone = true;
        }

        if (index == indexTodo && item.isDone == true) {
           item.isDone = false;
        }

        return item
      });
      this.saveToStorage();

    },
    saveToStorage(){
      localStorage.setItem('todos',JSON.stringify(this.todos))
    }
  },
  computed:{
    totalTodo() {
      return this.todos.length
    },
    progressBar(){
      const bar = []
      this.todos.map(function(v){
        if (v.isDone) {
          return bar.push(v.isDone)
        }
      })
      
      return (bar.length / this.todos.length) * 100
      // return this.bar.length
    }
  }
}

</script>

<template>
<div class="container" style="margin-top:20px">
  <div class="card">
    <div class="card-body">
      <h5 class="card-title">Simple Todo App</h5>
      <div class="row">
        <div class="col-10">
        <br>
          <input type="text" v-model="todo" v-on:keyup.enter="add" class="form-control">
        </div>
        <div class="col-2">
          <br>
          <button class="btn btn-success" @click="add">Add</button>
        </div>
      </div>
      <todos :todos="todos" @deleteTodo="deleteTodo" @doneTodo="doneTodo" />
      <br>
      <small>Total Todo : {{ totalTodo }}</small>
      <div class="progress mt-3">
        <div class="progress-bar" role="progressbar" v-bind:style="{ width: progressBar+'%' }" aria-valuenow="25" aria-valuemin="0" aria-valuemax="100">{{ progressBar }}%</div>
      </div>
    </div>
  </div>
</div>
</template>


