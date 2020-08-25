<template>
  <div id="app">
    <h1 v-if="todos.length">あなたがやることはあと{{remaining}}個です</h1>
    <h1 v-else>すべて終わりました！</h1>
    <ul>
      <li v-for="(todo,index) in todos" :key="todo.id" style="margin-bottom:5px">
        <input type="checkbox" v-model="todo.isDone">
        <span :class="{done:todo.isDone}">{{todo.title}}</span>
        <span 
          @click="deleteItem(index)"
          style="border:1px solid gray;border-radius:2px;margin-left:5px;cursor:pointer"
        >削除</span>
      </li>
    </ul>
    <button style="margin-bottom:20px" @click="purge">一括削除</button>
    <form @submit.prevent="addItem">
      <input type="text" v-model="newItem">
      <input type="submit" value="追加" style="margin-left:5px">
    </form>
  </div>
</template>

<script>

export default {
  data(){
    return{
      newItem:'',
      todos:[]
    }
  },
  watch:{
    todos:{
      handler(){
        localStorage.setItem('todos',JSON.stringify(this.todos));
      },
      deep:true
    }
  },
  mounted: function() {
      this.todos = JSON.parse(localStorage.getItem('todos')) || [];
    },
  methods:{
    addItem(){
      let item={
        title:this.newItem,
        isDone:false
      };
      this.todos.push(item);
      this.newItem='';
    },
    deleteItem(index){
      this.todos.splice(index,1);
    },
    purge(){
      this.todos = this.todos.filter(function(todo){
        return !todo.isDone;
      });
    }
  },
  // 算出プロパティを使ってみよう
  computed:{
    remaining(){
      let items=this.todos.filter(function(todo){
        // todoのisDoneがfalse，つまり残っているタスクをreturnする
        return !todo.isDone;
      });
      // その件数を出力
      return items.length;
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#app li{
  list-style: none;

}
#app li > span.done{
  text-decoration: line-through;
  color:#bbb;
}

</style>
