<!-- Component to display the contents on the current flash card -->
<template>
    <div id="card" v-on:click="flipCard">
        <!-- Only displays cards if at least one card was initialized -->
        <div v-if="addedFirstCard">
            <!-- Display card at the current index -->
            <p id="term" v-if="displayTerm">Term: {{ notes[index].term }}</p> 
            <p id="definition" v-if="displayDef">Definition: {{ notes[index].def }}</p>
        </div>
        <div v-else>
            <p>Please add a term and definition to begin.</p>
        </div>
    </div>
</template>

<script>
export default{
    props: ['term', 'def', 'addedFirstCard', 'notes', 'index'],
    data(){
        return{
            displayTerm: true,
            displayDef: false
        }
    },
    methods:{
        flipCard(){
            this.displayTerm = !this.displayTerm;
            this.displayDef = !this.displayDef;
        }
    }
}
</script>

<style scoped>
    p{
        color: white;
        text-align: center;
    }
    #card{
        background-color: rgb(48, 44, 75);
        padding: 100px;
        border-radius: 10px;
        width: 400px;
        height: 150px;
        margin-top: 50px;
        margin-right: auto;
        margin-left: auto;
    }
    @media only screen and (max-width: 700px){
        #card{
            width: 200px;
            height: 100px;
        }
    }
</style>