<template>
    <div class="backdrop">
        <div class="raceTitleContainer">
            <h3>{{race.title}}</h3>
            <p class="closeRace button" @click="closeRace">X</p>
        </div>
        <div class="division"></div>
        <!-- this screen allows users to place bets on dogs prior to the race commencing -->
        <PreRaceBets v-if="!raceCommenced" :dogsInRace="dogsInRace" :user="user" :race="race" @dog-not-found="closeRace" @begin-race="onBeginRace"/>
        <!-- visualizes the race and displays the results -->
        <CommenceRace v-if="raceCommenced" :dogsInRace="dogsInRace" :betList="betList" :raceLength="race.distance" @close-race="closeRace" @match-concluded="matchConcluded"/>
    </div>
</template>

<script>
import PreRaceBets from './PreRaceBets.vue';
import CommenceRace from './CommenceRace.vue';

export default {
    name: 'SelectedRace',
    components: {
        PreRaceBets,
        CommenceRace
    },
    data() {
        return {
            dogList: [],
            dogsInRace: [],
            betList: [],
            raceCommenced: Boolean,
            placedBet: Boolean
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
        onBeginRace(betList) {
            if(betList && betList.length){
                //deduct bets from balance
                var totalBetAmount = 0
                for(const bet of betList){
                    totalBetAmount += bet.amount
                }
                
                this.user.balance -= totalBetAmount
                //update user in db
                this.updateUser()
                this.betList = betList
                this.placedBet = true;
            } else {
                this.betList = []
                this.placedBet = false
            }
            this.raceCommenced = true
        },
        matchConcluded(winnings){
            if(this.placedBet){
                if(winnings > 0) {
                    this.user.balance += winnings * 2;
                    this.user.wins += 1  
                } else {
                    this.user.losses += 1
                }

                //update user in db
                this.updateUser()
            }
        },
        updateUser() {
            this.$emit('update-user')
        },
        async updateDogs(winningDog){
            this.dogsInRace.forEach((dog) => {
                if(dog.id === winningDog.id){
                    dog.wins++;
                } else {
                    dog.losses++;
                }
            })
        },
        async fetchDogs() {
            const req = await fetch('api/dogs')
            const dogData = await req.json();
            return dogData
        }
    },
    async created() {
        this.raceCommenced = false;
        this.dogList = await this.fetchDogs()
        this.race.dogs.forEach((dog)=> {
            const res = this.dogList.find((pup) => pup.id === dog)
            if(!res){
                alert('ERROR: '+ this.dogID +' not found.')
                this.$emit('dog-not-found');
                return
            }
            this.dogsInRace.push(res)
        })
        console.log('done')
    },
    emits: ['close-race', 'dog-not-found', 'update-user']
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

.backdrop {
    background-color: black;
}

</style>