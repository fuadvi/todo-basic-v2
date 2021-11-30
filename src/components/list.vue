<template>
  <div style="margin-top:20px">
    <ul class="list-group">
      <li v-for="todo in todos" :key="todo" class="list-group-item">{{ todo }}</li>
    </ul>
  </div>
</template>

<script>
export default {
  props:{
    todos:{
      type:Array,
      default:[]
    }
  }
}
</script>