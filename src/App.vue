<template>
  <div id="app">
    <GoalInput />
    <GoalList />
    <LoginBox v-show="false" />
  </div>
</template>

<script>
import GoalList from '@/components/GoalList.vue'
import GoalInput from '@/components/GoalInput.vue'
import LoginBox from '@/components/LoginBox.vue'

export default {
  components: {
    GoalList,
    GoalInput,
    LoginBox
  }
}
</script>

<style>
body {
  background-color: black;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #ffffff;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}

div {
  margin: 10px auto;
}
</style>
