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
  methods: {
    add(){
      return this.todos.push(this.todo);
    }
  },
}

</script>

<template>
<div class="container" style="margin-top:20px">
  <div class="card">
    <div class="card-body">
      <h5 class="card-title ">Simple Todo App</h5>
      <div class="row">
        <div class="col-10">
          <input type="text" v-model="todo" class="form-control">
        </div>
        <div class="col-2">
          <button class="btn btn-success" @click="add">Add</button>
        </div>
      </div>
      <todos :todos="todos"/>
    </div>
  </div>
</div>
</template>


