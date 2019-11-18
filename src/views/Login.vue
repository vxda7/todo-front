<template>
  <div class="login">
    <h1>로그인 페이지입니다.</h1>
    <LoginForm/>
  </div>
</template>

<script>
import LoginForm from '../components/LoginForm'
export default {
  name: 'Login',
  data(){
    return {
      
    }
  },
  components: {
    LoginForm
  }
}
</script>

<style>

</style>