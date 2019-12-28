<template>
  <div class="todo-item">
    <div style="display:flex">
<!-- check  -->
              <input
                type="checkbox"
                class="box-check"
                v-model="active"
                @change="editedLabel"
              >
              <div
                class="todo-item-label"
                :class="{ acted :active }"
                v-if="!status"
                @dblclick="editTitle"
              >

               {{ title }}
              </div>
              <!-- Edit lai name  -->
              <input
                v-else
                class="todo-item-edit"
                @blur="editedLabel"
                @keyup.enter="editedLabel"
                type="text"
                v-model="title"
                v-focus
                @keyup.esc="cancelEdit"
              >

            </div>
            <div class="remove-list">
              <button @click="removeApp(index)">
                x
              </button>
            </div>
  </div>
</template>

<script>
export default {
  name:'todoItem',
  // Prop khai bao cac du lieu truyen tu component cha ma component con can su dung
  props:{
    /*
      post:{
        type: {
          Object,Array,Number , Bool,....
        },
        required:true,
      }
    */
    todo:{
      type:Object,
      required:true,
    },
    index:{
      type:Number,
      required:true,
    },
    checkAll:{
      type:Boolean,
      required:true
    }
  },
  // 
   watch:{
    checkAll(){
      if(this.chechAll){
        this.active = true;
      } else {
        this.active = this.todo.active;
      }
    }
  },
  data(){
    return {
      id:this.todo.id,
      title:this.todo.title,
      active:this.todo.active,
      status:this.todo.status,
      beforeCache:'',
    }
  },
  methods:{
    removeApp(){
      this.$emit('removeApp',this.index);
    },
    editTitle(){
      this.beforCache = this.title;
      this.status = true;
    },
    editedLabel(){
      if(this.title.split('').length === 0){
        this.title = this.beforCache;
      }
      this.status = false;
      this.$emit('finishEdit',{
        'index':this.index,
        'todo':{
          'id':this.id,
          'title':this.title,
          'active':this.active,
          'status':this.status,
        }
      })
    },
    cancelEdit(){
      this.title = this.beforCache;
      this.status = false;
    },
  },
  directives: {
    focus: {
    // định nghĩa cho directive
      inserted: function (el) {
        el.focus()
      }
    }
  },
}
</script>

<style>

</style>
