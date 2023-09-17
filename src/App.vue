<template>
  <!-- <nav>
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link>
  </nav> -->
  <header>
    <h1>The Dog Race</h1>
  </header>
  <router-view/>
  <footer>Made by David Akridge</footer>
</template>

<script>
  export default {
    name: 'App',
  }
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;900&display=swap');

html {
  font-family: 'Poppins', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #E0E0CE;
  background-color: #222222;
}

header h1 {
  font-weight: bold;
  font-size: 100px;
  margin: 0px;
  color: #F2AF29;
  text-shadow: 10px 5px 5px black;
  text-decoration:underline;
}

footer {
  margin: 10px;
}

.button, button {
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

.button:hover, button:hover {
  background: rgb(179, 1, 1);
}

.button:active, button:active {
  background: rgb(90, 0, 0);
  color: white;
}

.division{
    border: 1px solid #E0E0CE;
}
</style>
