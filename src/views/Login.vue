<template>
  <form class="login" @submit="onSubmit">
    <h1>User ID</h1>
    <input type="text" class="form" placeholder="*****" v-model="uid"/>
    <input type="submit" class="btn btn-block"/>
  </form>
</template>

<script>
export default {
  name: 'Login',
  props: {
    user: Object
  },
  data() {
    return {
      uid: '',
    }
  },
  methods: {
    onSubmit(e){
      e.preventDefault();

      if(!this.uid) {
        alert('Please enter your user ID')
        return
      } 

      this.$emit('submit-uid', this.uid)

      this.uid = '';
    },
    //emits: ['submit-uid']
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.login {
  max-width: 300px;
  margin: auto;
  border: 5px ridge rgb(117, 117, 117);
  background-color: #2e2e2e;
  text-align: center;
  display: flex;
  flex-flow: column;
  align-items: center;
  padding: 10px;
  
}

h1 {
  margin:0px;
  text-shadow: -3px 0 black, 0 3px black, 3px 0 black, 0 -2px black;
}
.form {
  border-radius: 5px;
  font-size: 20px;
  font-weight: bold;
  text-align: center;
}

.btn {
  background: darkred;
  color: #E0E0CE;
  padding: 10px 20px;
  margin: 10px;
  border-radius: 5px;
  cursor: pointer;
  text-transform: uppercase;
  font-size: 15px;
  font-weight: bold;
  box-shadow: 5px 5px 5px black;
  border: 2px solid rgb(107, 0, 0);
  text-shadow: -3px 0 black, 0 3px black, 3px 0 black, 0 -3px black;
}

.btn-block {
  display: block;
}

.btn:hover {
  background: rgb(179, 1, 1);
}

.btn:active {
  background: rgb(90, 0, 0);
  color: white;
}
</style>
