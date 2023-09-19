<template>
    <div class="commencementContainer">
        <div>Race Commenced!</div>
        <div class="raceContainer" v-for="(dog,index) in dogsInRace" :key="dog">
            <div class="track">
                <div class="dog" :id="dog.id" :style="{'backgroundColor': getColor(index), 'animationDuration': dog.timeToComplete+'s'}">{{dog.id[0]}}</div>
            </div>
        </div>
        <div class="winner banner" v-if="raceConcluded">-{{ winningDog.id }} is the winner!-</div>
        <div class="division segment"></div>
        <div style="text-decoration: underline;">Bets Placed:</div>
        <div class="betContainer" v-for="bet in betList" :key="bet[0]">
            <div :class="['bet', (raceConcluded && bet.dog === winningDog.id ? 'winner' : '')]">
                {{ bet.dog }} at ${{ bet.amount }}
            </div>
        </div>
        <div class="buttonContainer" style="width: 100%">
            <button @click="$emit('close-race')">Return</button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'CommenceRace',
    props: {
        dogsInRace: Array,
        betList: Array,
        raceLength: Number
    },
    data () {
        return {
            raceConcluded : false,
            sortedDogRace : [],
            winningDog: {}
        }
    },
    created() {
        for(const dog of this.dogsInRace){
            
            //create some variance in the dogs speed so each race is unique
            var variedSpeed= dog.speed * (0.5 + Math.random())
            //t= d/v+v/(2a)
            dog.timeToComplete = (this.raceLength/variedSpeed) + (variedSpeed/(2*dog.acceleration))
        }
    },
    mounted() {
        //find the fastest dog and display the race report when they finish
        this.sortedDogRace = this.dogsInRace.toSorted((a, b) => a.timeToComplete - b.timeToComplete);
        this.winningDog = this.sortedDogRace[0];

        const dogElement = document.getElementById(this.winningDog.id);

        if (dogElement) {
            dogElement.addEventListener('animationend', () => {
                //when the winning dog wins the race, conclude the race, calculate winnings and emit upwards

                this.raceConcluded = true;

                //calculate winnings
                var winnings = 0
                for(const bet of this.betList) {
                    if(bet.dog === this.winningDog.id){
                        winnings += bet.amount
                    }
                }

                this.$emit('match-concluded', winnings);
            })
        }
    },
    methods: {
        getColor(index){
            const colors = ['red', 'blue', 'green', 'goldenrod', 'pink', 'purple', 'orange']
            return colors[index%colors.length]
        }
    },
    emits: ['close-race', 'match-concluded']
}
</script>

<style scoped>
.division {
    margin: 40px 10px;
}

.segment {
    border: 2px solid #64522d;
    margin: 10px;
}

.banner {
    font-size: xx-large;
}

.betContainer {
    color: gold;
    display: inline-flex;
}

.bet {
    margin: 5px;
}

.winner {
    color: green;
}

.loser {
    color: red;
}

.raceContainer {
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 20px;
    margin-top: 30px;
}

.track {
    width: 100%;
    height: 4px;
    background-color: #aa9b7d;
}

.dog {
    font-weight: bold;
    width: 20px;
    height: 20px;
    position: relative;
    top: -8px;
    display:flex;
    justify-content: center;
    align-items: center;
    animation: moveRight linear;
    animation-fill-mode: forwards;
}

@keyframes moveRight {
    0% {
        left: 0;
    }
    100% {
        left: 97%;
    }
}
</style>