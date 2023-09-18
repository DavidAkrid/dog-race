<template>
    <div class="panel">
        <RacesContainer v-if="!raceSelectedBool" :races="races" @race-selected="raceSelected"/>
        <SelectedRace v-if="raceSelectedBool" :race="selectedRace" :user="user" @close-race="onCloseRace" @update-user="$emit('update-user')"/>
    </div>
</template>

<script>
import RacesContainer from './RacesContainer.vue';
import RaceIcon from './RaceIcon.vue';
import SelectedRace from './SelectedRace.vue';

export default {
    name: 'RacePanel',
    components: {
        RacesContainer,
        RaceIcon,
        SelectedRace
    },
    props: {
        user: Object
    },
    data () {
        return {
            races: [],
            dogs: [],
            raceSelectedBool: false,
            selectedRace: Object
        }
    },
    methods: {
        raceSelected(race){
            this.raceSelectedBool = true;
            this.selectedRace = race;
        },
        onCloseRace(){
            this.raceSelectedBool = false;
            this.selectedRace = {};
        },
        async fetchRaces() {
            const req = await fetch('api/races')
            const raceData = await req.json()
            return raceData
        }
    },
    async created() {
        this.races = await this.fetchRaces()
    }
}
</script>

<style scoped>
.panel {
    display: flex;
    flex-direction: column;
    border: 5px groove rgb(106, 106, 106);
    border-radius: 5px;
    background-color: #272727;
    flex-grow: 1;
    margin: 2px;
}
</style>