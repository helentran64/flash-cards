<!-- Component to enter term and definition of a flash card -->
<template>
    <div class="inputContainer">
        <div>
        <p class="add">Add card</p>
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
        <div class="note" v-for="note in notes">
            <p>Term: {{ note.term }}</p>
            <p> Definition: {{ note.def }}</p>
        </div>
    </div>
</template>

<script>
export default{
    emits: ['info'], // Initialize emits
    data(){
        return{
            term: "",
            def: "",
            notes: [],
        }
    },
    methods:{
        // Once user submits, their info will be sent to Home.vue (parent) so it can be sent to FlashCard.vue for display
        submitInfo(){
            this.term = this.$refs.term.value;
            this.def = this.$refs.def.value;
            this.$emit('info', this.term, this.def);
            // Push to the notes array to be displayed
            this.notes.push({term: this.term, def: this.def});
        }
    }
}
</script>

<style scoped>
    p{
        color: white;
    }
    .add{
        font-size: 20px;
    }
    .inputContainer{
        margin-right: 120px;
        margin-left: 120px;
    }
    .inputField{
        background-color: rgb(48, 44, 75);
        padding: 20px;
        border-radius: 10px;
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
    .note{
        background-color: rgb(48, 44, 75);
        border-radius: 5px;
        width: 500px;
        margin-right: auto;
        margin-left: auto;
        padding: 20px;
        margin-top: 20px;
        margin-bottom: 20px;
    }
</style>