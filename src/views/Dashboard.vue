<template>
    <div class="dashContainer">
        <div class="division"></div>
        <h1>Welcome to the Races</h1>
        <div class="division bottom"></div>
        <div class="panelContainer">
            <UserPanel :user="user" @log-out="$emit('log-out')"/>
            <RacePanel :user="user" @update-user="$emit('update-user')"/>
        </div>
    </div>
</template>

<script>
import UserPanel from '../components/UserPanel.vue';
import RacePanel from '../components/RacePanel.vue';

export default {
    name: 'Dashboard',
    components: {
        UserPanel,
        RacePanel
    },
    props: {
        user: Object
    },
    methods: {
        logOutUser(){
            this.$emit('log-out')
        }
    }
}
</script>

<style scoped>


.dashContainer{
    display: flex;
    flex-direction: column;
    margin: 5px;
    padding: 5px;
    min-width: 600px;
    border: 5px ridge rgb(117, 117, 117);
    background-color: #2e2e2e;
    border-radius: 5px;
}



.panelContainer {
    display: flex;
    justify-content: space-evenly;
}

h1 {
    margin: 0px;
}

.division {
    width: 95%;
    margin: auto;
    border: 3px double #E0E0CE;
}

.bottom {
    margin-bottom: 10px;
}

</style>