<template>
    <div class="dogContainer">
        <h4>{{ dogData.id }}</h4>
        <p>Speed: {{ dogData.speed }}</p>
        <p>Accel: {{ dogData.acceleration }}</p>
    </div>
</template>

<script>
export default {
    name: 'Dog',
    props: {
        dogData: Object
    },
    data() {
        return {
            dog: Object
        }
    }
}
</script>

<style scoped>
.dogContainer {
    text-align: left;
    border: 4px groove gold;
    border-radius: 5px;
    min-width: 100px;
    max-height: 80px;
    padding: 5px;
    margin: 2px;
    cursor: pointer;
    user-select: none;
}

.dogContainer:not(.selected):hover {
    border: 4px groove goldenrod;
}

.selected {
    border: 4px groove blue;
}


h4 {
    margin: 0px 5px;
}

p {
    color: gold;
    padding-left: 15px;
    margin: 0px;
    line-height: 110%;
}
</style>