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
                addedFirstCard: false
            }
        }
    }
</script>

<template>
    <!-- Inject Card and Input components onto the Homepage -->
    <!-- Input retrieved from Input.vue will be passed to FlashCard.vue -->
    <Card v-bind:term="this.term" v-bind:def="this.def" v-bind:addedFirstCard="this.addedFirstCard"/>
    <Input @info="(term, def, addedFirstCard) =>{
        this.term = term;
        this.def = def;
        this.addedFirstCard = addedFirstCard;
    }
    "/>
</template>

<style scoped>
    
</style>