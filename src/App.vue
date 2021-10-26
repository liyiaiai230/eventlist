<template>
  <div id="app">
    <div class="todo-container">
      <div class="todo-wrap">
        <MyHeader @addTodo="addTodo"/>
        <MyList :todos="todos" :checkTodo="checkTodo" :remove="remove"/>
        <MyFooter :todos="todos" @checkAllTodo="checkAllTodo" @clearAllTodo="clearAllTodo"/>
      </div>
    </div>
  </div>
</template>

<script>
import MyFooter from "./components/MyFooter";
import MyList from "./components/MyList";
import MyHeader from "./components/MyHeader";

export default {
  name: 'App',
  components: {
    MyFooter,
    MyList,
    MyHeader,
  },
  data(){
    return{
      todos:JSON.parse(localStorage.getItem('todos')) || []
    }
  },
  methods:{
    addTodo(todoObj){
      this.todos.unshift(todoObj)
    },
    checkTodo(id){
      this.todos.forEach((todo)=>{
        if (todo.id === id ) todo.done = !todo.done
      })
    },
    remove(id){
     this.todos= this.todos.filter((todo)=>{
        return todo.id !== id
      })

    },
    clearAllTodo(){
      this.todos = this.todos.filter((todo)=>{
        return ! todo.done

      })
    },
    checkAllTodo(done) {
       this.todos.forEach((todo)=> {
        todo.done = done
      })
    }
  },
  watch:{
   todos:{
     deep:true,
    handler(value){
      localStorage.setItem('todos',JSON.stringify(value))
    }
   }
  }
}
</script>

<style>

body {
  background: #fff;
}
.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255,0.2), 0 1px rgba(0,0,0,0.05);
  border-radius: 4px;
}
.btn-danger {
  color: #ffffff;
  background-color:#da4f49 ;
  border: 1px solid #bd362f;
}
.btn-danger:hover {
  color: #ffffff;
  background-color:#bd362f ;
}
.btn:focus {
  outline: none;
}
.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap{
  padding: 10px;
  border: 1px solid #dddddd;
  border-right:5px ;

}
</style>

