<template>
  <div class="todo-list" id="todolist">
       <input
          type="text"
          v-model="todoTitle"
          placeholder="What is title of Todolist"
          @keyup.enter="addTodolist"
       >
        <div class="list">
          <todo-item
            v-for="(todo,index) in todoListFilters"
            v-bind:key="index"
            class="result"
            :todo="todo"
            :index="index"
            @removeApp="removeApp"
            @finishEdit="finishEdit"
            :checkAll="!anyChecked"
          >
          </todo-item>
            <div class="check-all">
              <label >
                <input type="checkbox"
                  :checked="!anyChecked"
                   @change="allChangeItem"

                   > Check All
              </label>
              <div class="filter">
                <button
                  :class="{active: filter == 'all'}"
                  @click="filter = 'all'"
                >
                All
                </button>
                <button

                  :class=" {active: filter == 'active'}"
                  @click="filter ='active'"
                  >
                  Active
                </button>
                <button
                  :class="{active: filter == 'completed'}"
                  @click=" filter='completed' "
                >
                    Completed
                </button>
              </div>
              <button @click="clearAll">Clear All</button>
              <p>{{ result }} item left</p>
            </div>
        </div>
  </div>
</template>

<script>
import '../styles/todolist.css'
import todoItem from './todoItem'
export default {
  name:'todolist',
 
  data(){
    return{
      todoTitle:'',
      idNumber:3,
      beforCache:'',
      filter:'all',
      todoLists:[
        {
          id:1,
          title:'The fisrt name todolist',
          active:false,
          status:false
        },
      {
          id:2,
          title:'The second name todolist',
          active:false,
          status:false
        }
      ]
    }
  },
  methods:{
    // Them thuoc tinh vao trong list todoLists

    addTodolist(){
      if(this.todoTitle.split('').length !== 0)
      {
        this.todoLists.push({
        id : this.idNumber,
        title : this.todoTitle,
        active : false,
        status: false
        })
      };
      this.todoTitle = '';
      this.idNumber++;
    },
    // Xoa thuoc item trong list todoLists => truyen vao index
    removeApp(index){
      this.todoLists.splice(index,1)
    },
    // Edit lai thuoc tinh trong todoItem

    allChangeItem(){
      this.todoLists.forEach((todo)=>todo.active = event.target.checked);
    },
    clearAll(){
      this.todoLists.forEach((todo)=>{
        todo.active = false;
      })
    },
    finishEdit(data){
      this.todoLists.splice(data.index,1,data.todo);
    }
  },

  computed:{
    result(){
      return this.todoLists.filter(todo => !todo.active).length;
    },
    anyChecked(){
      return this.result != 0;
    },
    todoListFilters(){
      if(this.filter =='all'){
        return this.todoLists;
      } else if( this.filter == 'active'){
        return this.todoLists.filter(todo => !todo.active);
      } else if(this.filter == 'completed'){
        return this.todoLists.filter(todo => todo.active);
      }
      return this.todoLists;
    }
  },
  components:{
    'todo-item':todoItem,
  },
  watch:{

  }
}
</script>
