<template>
    <Login v-if="!loggedIn" @submit-id="submitID" />
    <Dashboard v-if="loggedIn" :user="user" @log-out="logOutUser" @update-user="updateUser"/>
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
    },
    methods: {
      async submitID(id){
        const req = await fetch(`api/users/${id}`)
        this.user = await req.json();

        this.loggedIn = true;

      },
      logOutUser(){
        this.loggedIn = false
        this.user={}
      },
      async updateUser() {
        const req = await fetch(`api/users/${this.user.id}`, {
          method: "PUT",
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(this.user),
        })

        req.status === 200 ? console.log('update success') : alert('error updating user info')
      }
    }
}
</script>