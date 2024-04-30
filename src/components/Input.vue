<!-- Component to enter term and definition of a flash card -->
<template>
    <div class="inputContainer">
        <div id="controls" v-if="addedFirstCard">
            <input type="button" value="previous" v-on:click="goBack">
            <input type="button" value="next" v-on:click="goForward">
        </div>
        <div id="tracker" v-if="addedFirstCard">
            <!-- Tracks which card you are currently on -->
            <p>{{ index+1 }} / {{ getCardCount }}</p>
        </div>
        <div>
        <p id="addHeading">Add card</p>
        </div>
        <div class="inputField">
            <div class="submitSection">
                <label for="term">Term</label>
                <input type="text" id="term" ref="term">
                <label for="def">Definition</label>
                <input type="text" id="def" ref="def">
                <input type="button" value="submit" id="submit" v-on:click="submitInfo">
            </div>
        </div>
    </div>

    <!-- Displays all the notes submitted -->
    <div class="listOfNotes">
        <p id="cardsHeading" v-if="addedFirstCard">Cards in this set ({{ getCardCount }})</p>
        <p></p>
        <div class="note" v-for="note in notes">
            <p>Term: {{ note.term }}</p>
            <p> Definition: {{ note.def }}</p>
        </div>
    </div>
</template>

<script>
export default{
    emits: ['info', 'back', 'forward'], // Initialize emits
    data(){
        return{
            index: 0,
            term: "",
            def: "",
            notes: [],
            addedFirstCard: false, // Updates to true if first card is added to notes array
        }
    },
    computed: {
        // Get the number of cards in the notes array
        getCardCount(){
            return this.notes.length
        }
    },
    methods:{
        // Once user submits, their info will be sent to Home.vue (parent) so it can be sent to FlashCard.vue for display
        submitInfo(){
            this.term = this.$refs.term.value;
            this.def = this.$refs.def.value;
            // Push only unique terms to the notes array for display
            let duplicate = this.hasDuplicates({term: this.term, def: this.def});
            if (!duplicate){
                this.notes.push({term: this.term, def: this.def});
            }
            // Set addedFirstCard to true to make the corresponding text visible.
            this.addedFirstCard = true;
            // Send information to Home.vue
            this.$emit('info', this.term, this.def, this.addedFirstCard, this.notes);
        },
        // Checks if the notes array have duplicates
        hasDuplicates(newNote){
            let current = newNote.term;
            for (let i = 0; i < this.notes.length; i++){
                if (this.notes[i].term == current){
                    return true;
                }
            }
            return false;
        },
        goBack(){
            if (this.index > 0){
                this.index -= 1;
                // Send information to Home.vue
                this.$emit('back', this.index);
            }
        },
        goForward(){
            if (this.index < this.notes.length-1){
                this.index += 1;
                // Send information to Home.vue
                this.$emit('forward', this.index);
            }
        }
    }
}
</script>

<style scoped>
    p{
        color: white;
    }
    #controls{
        max-width: fit-content;
        margin-left: auto;
        margin-right: auto;
        padding: 10px;

    }
    input{
        padding: 5px;
        margin: 2px;
    }
    #tracker{
        max-width: fit-content;
        margin-left: auto;
        margin-right: auto;
        padding: 10px;
    }
    #addHeading{
        font-size: 20px;
    }
    .inputContainer{
        width: 600px;
        margin-left: auto;
        margin-right: auto;
    }
    .inputField{
        background-color: rgb(48, 44, 75);
        padding: 20px;
        border-radius: 10px;
        width: 560px;
        margin-left: auto;
        margin-right: auto;
    }
    label{
        padding: 5px;
        color: white;
    }
    #submit{
        margin: 5px;
    }
    .submitSection{
        max-width: fit-content;
        margin-right: auto;
        margin-left: auto;
    }
    .listOfNotes{
        width: 600px;
        margin-left: auto;
        margin-right: auto;
    }
    #cardsHeading{
        font-size: 20px;
    }
    .note{
        background-color: rgb(48, 44, 75);
        border-radius: 10px;
        width: 560px;
        padding: 20px;
        margin-right: auto;
        margin-left: auto;
        margin-top: 20px;
        margin-bottom: 20px;
    }
</style>