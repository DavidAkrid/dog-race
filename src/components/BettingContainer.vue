<template>
    <div class="betPanel">
        <PlaceBet :dogList="dogList" :user="user" @place-bet="onPlaceBet"/>
        <div class="division"></div>
        <div>Bets Placed:</div>
        <div class="bet">
            <div  v-for="bet in betList" :key="bet[0]">
                {{ bet.dog }} at ${{ bet.amount }}
                <span class="cancelBet" @click="removeBet(bet.dog)">x</span>
            </div>
        </div>
        <button @click="$emit('begin-race', this.betList)">Begin Race</button>
    </div>
</template>

<script>
import PlaceBet from './PlaceBet.vue';

export default {
    name: 'BettingContainer',
    components: {
        PlaceBet
    },
    props: {
        dogList: Array,
        user: Object
    },
    data() {
        return {
            betList: []
        }
    },
    methods: {
        onPlaceBet(dog, betAmount) {
            //check if total bet $ is greater than balance, if so, deny bet.

            var totalBetAmount = betAmount
            
            for(const currBet of this.betList){
                totalBetAmount += currBet.amount
            }

            if(totalBetAmount > this.user.balance){
                alert('Total Bet Amount Exceeds User Balance!')
                return
            }

            
            //search the betlist to see if dog has already been bet on.
            const existingDog = this.betList.find((tempBet) => tempBet.dog === dog)

            if(existingDog){
                //if dog exists, add bet to amount.
                existingDog.amount += betAmount
            } else {
                //else add a new bet
                const bet = {
                    dog: dog,
                    amount: betAmount
                }
                this.betList.push(bet)
            }
        },
        removeBet(dog){
            this.betList = this.betList.filter((bet)=> bet.dog !== dog)
        }
    },
    emits: ['begin-race']
}
</script>

<style scoped>
.betPanel {
    background-color: black;
    border: 4px groove #F2AF29;
    border-radius: 5px;
    padding: 10px;
    width: 175px;
    /* min-width: 175px; */
}

.bet {
    color: gold;
    min-height: 25px;
    border: 4px groove #F2AF29;
    border-radius: 5px;
}

.division{
    border: 1px solid #F2AF29;
}

.cancelBet{
    color: red;
    opacity: .95;
    cursor: pointer;
}
</style>