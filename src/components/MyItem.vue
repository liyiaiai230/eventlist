<template>
  <div>
    <li>
      <label>
        <input type="checkbox" :checked="todo.done" @change="handleCheck(todo.id)"/>
        <span>{{todo.title}}</span>
      </label>
      <button class="btn btn-danger" @click="handleDelete(todo.id)">删除</button>
    </li>
  </div>
</template>

<script lang="js">
export default {
  name: 'MyLtem',
  props:['todo','checkTodo','remove'],
  methods:{
    handleCheck(id) {
      this.checkTodo(id)
    },
    handleDelete(id) {
     if (confirm('确认删除')){
       this.remove(id)
     }
    }
  },
};
</script>

<style scoped>
li{
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px ;
  border: 1px solid #ddd;

}
li label {
  float: left;
  cursor: pointer;
}
li label li input {
  vertical-align:middle ;
  margin-right: 6px;
  position: relative;
  top: -1px;
}
li button {
  float: right;
  display: none;
  margin-top:3px ;
}
li:before{
  content: initial;
}
li:last-child {
  border-bottom: none;
}
li:hover{
  background-color: #dddddd;
}
li:hover button{
  display: block;
}
</style>