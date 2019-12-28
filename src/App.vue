<template>
  <div id="app">
    <img src="./assets/logo.png">
    <todolist/>
  </div>
</template>

<script>
import todolist from './components/Todolist'
export default {
  name: 'app',
  data () {
    return {

    }
  },
  components:{
    todolist,
  }
}
</script>
<style>

</style>
