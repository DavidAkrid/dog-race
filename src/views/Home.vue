<template>
    <Login v-if="!loggedIn" @submit-uid="submitUID" />
    <Dashboard v-if="loggedIn" :user="user" @log-out="logOutUser"/>
</template>

<script>
import Login from './Login.vue';
import Dashboard from './Dashboard.vue';


export default {
    name: 'Home',
    components: {
        Login,
        Dashboard
    },
    data() {
      return {
        user: [],
        loggedIn: Boolean
      }
    },
    async created() {
        this.loggedIn = false

        this.users = await this.fetchUsers();
    },
    methods: {
      async fetchUsers(){
        const req = await fetch('api/users')

        const userData = await req.json();

        return userData
      },
      submitUID(uid){
        //locate user
        const res = this.users.find((user) => user.uid === uid)

        if(!res) {
          alert('User not found.')
          return
        }

        this.user = res
        this.loggedIn = true;

        //this.$router.push('/dashboard')
      },
      logOutUser(){
        this.loggedIn = false
        this.user={}
      }
    }
}
</script>