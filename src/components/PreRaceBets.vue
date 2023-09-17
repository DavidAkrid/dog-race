<template>
    <div class="selectedRaceContainer">
            <div>
                <h4>Dogs:</h4>
                <div class="raceDogsDetails">
                    <Dog :dogData="dog" v-for="dog in dogsInRace" :key="dog.name" @dog-not-found="closeRace"/>
                </div>
            </div>
            <BettingContainer :dogList="race.dogs" :user="user" @begin-race="$emit('begin-race')"/>
        </div>
</template>

<script>
import Dog from './Dog.vue';
import BettingContainer from './BettingContainer.vue';

export default {
    name: 'PreRaceBets',
    props: {
        race: Object,
        user: Object
    },
    components: {
        Dog,
        BettingContainer,
    },
    data() {
        return {
            dogList: [],
            dogsInRace: []
        }
    },
    async created() {
        this.dogList = await this.fetchDogs()
        this.race.dogs.forEach((dog)=> {
            const res = this.dogList.find((pup) => pup.name === dog)
            if(!res){
                alert('ERROR: '+ this.dogID +' not found.')
                this.$emit('dog-not-found');
                return
            }
            this.dogsInRace.push(res)
        })
    },
    methods: {
        closeRace() {
            this.$emit('close-race')
        },
        async fetchDogs() {
            const req = await fetch('api/dogs')
            const dogData = await req.json();
            return dogData
        }
    },
    emits: ['dog-not-found', 'begin-race']
}
</script>

<style>
.selectedRaceContainer {
    margin: 10px;
    display: flex;
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