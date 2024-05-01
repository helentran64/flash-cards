<!-- Homepage -->

<script>
    import Card from '../components/FlashCard.vue'
    import Input from '../components/Input.vue'
    export default {
        components: {Card, Input},
        data(){
            return{
                term: "",
                def: "",
                addedFirstCard: false,
                flashCards: [],
                index: 0
            }
        }
    }
</script>

<template>
    <!-- Inject Card and Input components onto the Homepage -->
    <!-- Input retrieved from Input.vue will be passed to FlashCard.vue -->
    <Card v-bind:term="this.term" v-bind:def="this.def" v-bind:addedFirstCard="this.addedFirstCard" v-bind:flashCards="this.flashCards" v-bind:index="this.index"/>
    <Input @info="(term, def, addedFirstCard, flashCards) =>{
        this.term = term;
        this.def = def;
        this.addedFirstCard = addedFirstCard;
        this.flashCards = flashCards;
    }"
    @back="(index) => {
        this.index = index;
    }"
    @forward="(index) => {
        this.index = index;
    }"/>
</template>

<style scoped>
    p{
        color: white;
    }
</style>