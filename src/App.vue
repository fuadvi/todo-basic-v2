<script>
import { reactive,ref,computed,onMounted,toRefs } from "vue";
import List from './components/list.vue'
export default {
components:{List},
  setup() {
    const todo = ref('')
    const todos = reactive({
      list: [],
    })

    onMounted(() => {
      const storage = JSON.parse(localStorage.getItem('todos'))
      if(storage)
        todos.list  = storage
    });

    const totalTodo = computed(() => {
      return todos.list.length
    }) 

    const progressBar = computed(() => {
       const bar = []
      todos.list.map(function(v){
        if (v.isDone) {
          return bar.push(v.isDone)
        }
      })
      
      return (bar.length / todos.list.length) * 100
    })

    const add = () => {
       todos.list.unshift({
         activity:todo.value,
         isDone: false
       });

       todo.value = '';

       saveToStorage();
    }

    const deleteTodo = indexTodo => {
      todos.list = todos.list.filter((item,index) => {
        if (index != indexTodo) {
          return item
        }
      });

      saveToStorage();
    }

    const doneTodo = indexTodo => {
      todos.list = todos.list.filter((item,index) => {
        if (index == indexTodo && item.isDone == false) {
          return item.isDone = true;
        }

        if (index == indexTodo && item.isDone == true) {
           item.isDone = false;
        }

        return item
      });
      saveToStorage();

    }

    const saveToStorage = () => {
      localStorage.setItem('todos',JSON.stringify(todos.list))
    }

    return {
      todo,
      ...toRefs(todos),
      totalTodo,
      progressBar,
      add,
      deleteTodo,
      doneTodo
    }

  },
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
      <list :todos="list" @deleteTodo="deleteTodo" @doneTodo="doneTodo" />
      <br>
      <small>Total Todo : {{ totalTodo }}</small>
      <div class="progress mt-3">
        <div class="progress-bar" role="progressbar" v-bind:style="{ width: progressBar+'%' }" aria-valuenow="25" aria-valuemin="0" aria-valuemax="100">{{ progressBar }}%</div>
      </div>
    </div>
  </div>
</div>
</template>


