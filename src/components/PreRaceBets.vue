<template>
    <div class="selectedRaceContainer">
            <div>
                <h4>Dogs:</h4>
                <div class="raceDogsDetails">
                    <Dog 
                        :dogData="dog" 
                        v-for="dog in dogsInRace" 
                        :key="dog.id" 
                        :class="{'selected':dog.id === selectedDogId}"
                        @dog-not-found="closeRace" 
                        @dblclick="dogSelectedFromPanel(dog.id)"
                    />
                </div>
            </div>
            <BettingContainer 
                :dogList="race.dogs" 
                :user="user"
                :selectedDogId="selectedDogId" 
                ref="bettingContainer"
                @dog-selected="dogSelectedFromDropdown"
                @begin-race="onBeginRace"
            />
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
            dogList: [],
            selectedDogId: null
        }
    },
    methods: {
        closeRace() {
            this.$emit('close-race')
        },
        onBeginRace(betList) {
            this.$emit('begin-race', betList)
        },
        dogSelectedFromPanel(dogId) {
            this.selectedDogId = dogId
            this.$refs.bettingContainer.externalDogSelected(dogId)
        },
        dogSelectedFromDropdown(selectedDogId){
            this.selectedDogId = selectedDogId
        }
    },
    emits: ['dog-not-found', 'dog-selected','begin-race']
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