<template>
        <h3>Place your bet!</h3>
        <div class="division"></div>
        <form @submit="onSubmit">
            <p>Select Dog:</p>
            <select class="dogDropdown" v-model="selectedDog" id="dropdown" @change="dogSelected(selectedDog)">
                <option :value="dog" v-for="dog in dogList" :key="dog">{{dog}}</option>
            </select>
            <p>Bet Amount:</p>
            <input type="number" name="bet" v-model="bet" placeholder="$$$">
            <input type="submit" class="submit button" value="Place Bet">
        </form>
</template>

<script>
export default {
    name: 'PlaceBet',
    data() {
        return {
            selectedDog: '',
            bet: null
        }
    },
    props: {
        dogList: Array,
        user: Object,
        selectedDogId: String
    },
    methods: {
        onSubmit(e){
            e.preventDefault();

            //basic verification to ensure dog is selected, bet is selected, and bet is valid based on balance
            if(!this.selectedDog){
                alert('Please enter a valid dog to bet on!');
                return
            }

            if(!this.bet){
                alert('Please enter an amount to bet with!');
                return
            }

            if(!this.verifyBet()){
                alert('Bet exceeds user balance!');
                return
            }
            this.$emit('place-bet', this.selectedDog, this.bet);
        },
        verifyBet(){
            return this.user.balance >= this.bet
        },
        //if the user selects the dog via the dog panel
        externalDogSelected(dogId){
            this.selectedDog = dogId
        },
        //if dog is selected via dropdown
        dogSelected(selectedDog){
            this.$emit('dog-selected', this.selectedDog)
        }
    },
    emits: ['place-bet', 'dog-selected']
}
</script>

<style scoped>

h3 {
    margin: 0px;
}

p {
    float:left;
    margin: 10px 0px;
}

.dogDropdown {
    width: 100%;
}

.submit {
  padding: 10px 20px;
  margin: 10px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 12px;
  text-shadow: -2px 0 black, 0 2px black, 2px 0 black, 0 -2px black;
}

.submit:hover {
  background: rgb(179, 1, 1);
}

.submit:active {
  background: rgb(90, 0, 0);
  color: lightgray;
}
.division{
    border: 1px solid #E0E0CE;
}
</style>