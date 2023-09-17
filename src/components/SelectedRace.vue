<template>
    <div class="backdrop">
        <div class="raceTitleContainer">
            <h3>{{race.title}}</h3>
            <p class="closeRace button" @click="closeRace">X</p>
        </div>
        <div class="division"></div>
        <PreRaceBets v-if="!raceCommenced" :user="user" :race="race" @dog-not-found="closeRace" @begin-race="onBeginRace"/>
        <CommenceRace v-if="raceCommenced"/>
    </div>
</template>

<script>
import Dog from './Dog.vue';
import PreRaceBets from './PreRaceBets.vue';
import CommenceRace from './CommenceRace.vue';

export default {
    name: 'SelectedRace',
    components: {
        Dog,
        PreRaceBets,
        CommenceRace
    },
    data() {
        return {
            dogList: [],
            dogsInRace: [],
            raceCommenced: Boolean
        }
    },
    props: {
        race: Object,
        user: Object
    },
    methods: {
        closeRace() {
            this.$emit('close-race')
        },
        async fetchDogs() {
            const req = await fetch('api/dogs')
            const dogData = await req.json();
            return dogData
        },
        onBeginRace() {
            console.log('success!')
            this.raceCommenced = true
        }
    },
    async created() {
        this.raceCommenced = false;

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
    emits: ['close-race', 'dog-not-found']
}
</script>

<style scoped>
.raceTitleContainer {
    display: flex;
    justify-content: space-between;
}

h3 {
    flex-grow: 1;
    margin: 1px;
}

h4 {
   margin: 0px; 
}

.division {
    width: 95%;
    border: 2px solid #64522d;
    margin: 2px auto;
}

.closeRace {
    float: right;
    background: darkred;
    color: white;
    padding: 0px 8px;
    margin: auto 5px;
    border-radius: 5px;
    cursor: pointer;
    box-shadow: 2px 2px 5px black;
}

.closeRace:hover {
    background: rgb(179, 1, 1);
}

.closeRace:active {
    background: rgb(90, 0, 0);
    color: lightgray;
}

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

.backdrop {
    background-color: black;
}

</style>