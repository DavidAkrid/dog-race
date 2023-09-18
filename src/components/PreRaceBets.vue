<template>
    <div class="selectedRaceContainer">
            <div>
                <h4>Dogs:</h4>
                <div class="raceDogsDetails">
                    <Dog :dogData="dog" v-for="dog in dogsInRace" :key="dog.id" @dog-not-found="closeRace"/>
                </div>
            </div>
            <BettingContainer :dogList="race.dogs" :user="user" @begin-race="onBeginRace"/>
        </div>
</template>

<script>
import Dog from './Dog.vue';
import BettingContainer from './BettingContainer.vue';

export default {
    name: 'PreRaceBets',
    props: {
        race: Object,
        user: Object,
        dogsInRace: Array
    },
    components: {
        Dog,
        BettingContainer,
    },
    data() {
        return {
            dogList: []
        }
    },
    methods: {
        closeRace() {
            this.$emit('close-race')
        },
        onBeginRace(betList) {
            this.$emit('begin-race', betList)
        }
    },
    emits: ['dog-not-found', 'begin-race']
}
</script>

<style>
.selectedRaceContainer {
    margin: 10px;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    max-width: none;
}

.raceDogsDetails {
    background-color: black;
    padding: 2px;
    display: flex;
    flex-wrap: wrap;
    max-width: 250px;
    justify-content: space-around;
    align-content: space-around;
    margin: 5px;
    min-width: 250px;
}
</style>