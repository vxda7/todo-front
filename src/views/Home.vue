<template>
  <div class="home">
    <TodoInput @createTodo="createTodo"/>
    <TodoList :todos="todos"/>
  </div>
</template>

<script>
import router from '../router'
import TodoList from '../components/TodoList'
import TodoInput from '@/components/TodoInput'
import jwtDecode from 'jwt-decode'
import axios from 'axios'

export default {
  name: 'home',
  components: {
    TodoList,
    TodoInput,
  },
  data(){
    return {
      todos: []
    }
  },
  methods: {
    checkLoggedIn(){
      this.$session.start()
      if (!this.$session.has('jwt')){
        //로그인 페이지로 리다이렉트
        router.push('/login')
      }
    },
    getTodos(){
      console.log("사용자의 투두목록 가져오기")
      this.$session.start()
      //const user_id = jwtDecode(this.$session.get('jwt')).user_id  한줄가능
      const token = this.$session.get('jwt')
      const decodedToken = jwtDecode(token)
      const user_id = decodedToken.user_id

      const requestHeader = {
        headers: {
          Authorization: "JWT " + token
          }
      }
      axios.get(`http://localhost:8000/api/v1/users/${user_id}/`, requestHeader)
      .then((res)=>{
        this.todos = res.data.todo_set
        })
      .catch((e)=>{console.log(e)})
    },
    createTodo(title){
      this.$session.start()
      const token = this.$session.get('jwt')
      const decodedToken = jwtDecode(token)
      const user_id = decodedToken.user_id
      const requestHeader = {
        headers: {
          Authorization: `JWT ${token}`
        }
      }
      const requestForm = new FormData()
      requestForm.append('user', user_id)
      requestForm.append('title', title)

      axios.post('http://localhost:8000/api/v1/todos/', requestForm, requestHeader)
      .then((res)=>{
        console.log(res)
        this.todos.push(res.data)
      })
      .catch((e)=>{console.log(e)})
    }
  },
  mounted: function(){
    this.checkLoggedIn()
    this.getTodos()
  }
}
</script>

<style>

</style>