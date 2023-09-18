<template>
    <div class="panel">
        <div class="divider"></div>
        <div class="userTitle">
            <h2>
                {{ user.id }}
            </h2>
            <div class="profilePic"></div>
        </div>
        <div class="divider"></div>
        <h4>
            Balance: 
            <span class="data">
                $ {{user.balance }}
            </span>
        </h4>
        <h4>
            Wins: 
            <span class="data">
                {{ user.wins }}
            </span>
        </h4>
        <h4>
            Losses: 
            <span class="data">
                {{ user.losses }}
            </span>
        </h4>
        <div class="divider"></div>
        <div @click="logOutClick" class="logoutButton">
            <button>Log out</button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'UserPanel',
    props: {
        user: Object
    },
    methods: {
        logOutClick(){
            this.$emit('log-out')
        }
    }
}
</script>

<style scoped>
.panel{
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    max-height: 275px;
    border: 5px groove rgb(106, 106, 106);
    border-radius: 5px;
    background-color: #272727;
    padding: 15px;
    margin: 2px;
    min-width: 200px;
    max-width: 225px;
    flex-grow: 5;
}

.userTitle {
    width: 100%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    text-transform: uppercase;
    font-size: 900;
}

.profilePic {
    height: 80px;
    width: 80px;
    background-image: url("../assets/img/profile.jpg");
    background-size: cover;
    background-repeat: no;
    border: 2px groove #F2AF29;
    margin: 10px;
}

.data {
    font-weight: normal;
    margin-left: 5px;
}

.logoutButton {
    display: flex;
    width: 100%;
    justify-content: center;
}

button {
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

button:hover {
  background: rgb(179, 1, 1);
}

button:active {
  background: rgb(90, 0, 0);
  color: lightgray;
}

.divider {
    border: 2px solid #64522d;
    width: 100%
}

h4 {
    margin: 5px;
}
</style>