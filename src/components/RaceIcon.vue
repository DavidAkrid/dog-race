<template>
    <div class="race" @click="onRaceIconClick">
        <h4>{{race.title}}</h4>
        <div class="division"></div>
        <div class="dogPanel">
            <p v-for="dog in race.dogs" :key="dog">{{dog}}</p>
        </div>
    </div>
</template>

<script>
export default {
    name: 'RaceIcon',
    props: {
        race: Object
    },
    data() {
        return {
            dogs: []
        }
    },
    methods: {
        onRaceIconClick(){
            this.$emit('race-selected', this.race)
        }
    }
}
</script>

<style scoped>
.race{
    background-color: black;
    border: 4px groove gold;
    border-radius: 5px;
    margin: 10px;
    padding: 10px;
    max-width: 120px;
    user-select: none;
    cursor: pointer;
}

.race:hover {
    border: 4px groove goldenrod;
}

h4 {
    margin: 3px;
    line-height: normal;
}

.dogPanel {
    display:flex;
    flex-wrap: wrap;
    justify-content: center;
    color: gold;
}

p {
    margin: 2px 5px;
}
</style>