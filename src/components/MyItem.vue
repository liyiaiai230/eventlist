<template>
  <div>
    <li>
      <label>
        <input type="checkbox" :checked="todo.done" @change="handleCheck(todo.id)"/>
        <span v-show="!todo.isEdit">{{todo.title}}</span>
        <input type="text"
        v-show="todo.isEdit" :value="todo.title" @blur="handleBlur(todo,$event)">
      </label>
      <button class="btn btn-danger" @click="handleDelete(todo.id)">删除</button>
      <button v-show="!todo.isEdit" class="btn btn-edit" @click="handleEdit(todo)">修改</button>
    </li>
  </div>
</template>

<script lang="js">
export default {
  name: 'MyLtem',
  props:['todo',],
  methods:{
    //勾选or取消勾选
    handleCheck(id) {
      this.$eventBus.$emit('checkTodo',id)
    },
    // 删除
    handleDelete(id) {
     if (confirm('确认删除')){
       this.$eventBus.$emit('remove',id)
     }
    },
    //编辑
    handleEdit(todo){
      if (todo.hasOwnProperty.call(todo,'isEdit')){
        todo.isEdit = true
      }else{
        this.$set(todo,'isEdit',true)
      }

    },
    //失去焦点回调
    handleBlur(todo,e){
      todo.isEdit= false
      if (!e.target.value.trim()) return alert('要输入任务！')
      this.$eventBus.$emit('updateTodo',todo.id,e.target.value)
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