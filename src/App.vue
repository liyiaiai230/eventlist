<template>
  <div id="app">
    <div class="todo-container">
      <div class="todo-wrap">
        <MyHeader @addTodo="addTodo"/>
        <MyList :todos="todos"/>
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
    //添加一个todo
    addTodo(todoObj){
      this.todos.unshift(todoObj)
    },
    //勾选or取消一个todo
    checkTodo(id){
      this.todos.forEach((todo)=>{
        if (todo.id === id ) todo.done = !todo.done
      })
    },
    //更新一个todo
    updateTodo(id,title){
      this.todos.forEach((todo)=>{
        if (todo.id === id ) todo.title = title
      })
    },
    //删除一个todo
    remove(id){
     this.todos= this.todos.filter((todo)=>{
        return todo.id !== id
      })

    },
   //全选or取消全部
    clearAllTodo(){
      this.todos = this.todos.filter((todo)=>{
        return ! todo.done

      })
    },
    //清除已完成的todo
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
  },
  mounted() {
    this.$eventBus.$on('checkTodo',this.checkTodo)
    this.$eventBus.$on('updateTodo',this.updateTodo)
    this.$eventBus.$on('remove', this.remove)
  },
  beforeDestroy() {
    this.$eventBus.$off('checkTodo')
    this.$eventBus.$off('remove')
    this.$eventBus.$off('updateTodo')
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
.btn-edit {
  color: #ffffff;
  background-color: #d0837d;
  border: 1px solid #878585;
  margin-right: 10px;
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

