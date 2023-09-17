<template>
    <h3>Races</h3>
    <div class="racesContainer">
        <div :key="race.title" v-for="race in races">
            <RaceIcon :race="race" @race-selected="raceSelected"/>
        </div>
    </div>
</template>

<script>
import RaceIcon from './RaceIcon.vue';

export default {
    name: 'RacesContainer',
    props: {
        races: Array,
        dogs: Array
    },
    components: {
        RaceIcon
    },
    methods: {
        raceSelected(race){
            this.$emit('race-selected', race)
        }
    },
    emits: ['race-selected']
}
</script>

<style scoped>

h3 {
    margin : 0px auto;
    margin-top: 10px;
}
.racesContainer {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: center;
    margin: 10px;
    border: 5px groove rgb(106, 106, 106);
    border-radius: 5px;
    background-color: #323232;
}

</style>