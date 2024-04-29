<template>
    <div id="card">
        <p id="term">Term: {{ term }}</p>
        <p id="definition">Definition: {{ def }}</p>
    </div>
    <div id="controls">
        <input type="button" value="previous">
        <input type="button" value="next">
    </div>
    <div id="tracker">
        <!-- Tracks which card you are currently on -->
        <p>{{ currentCardNumber }} / {{ totalCards }}</p>
    </div>
</template>

<script>
export default{
    props: ['term', 'def', 'sent'],
    data(){
        return{
            currentCardNumber: 0,
            totalCards: 0,
        }
    }
}
</script>

<style scoped>
    p{
        color: white;
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
    @media only screen and (max-width: 700px){
        #card{
            width: 200px;
            height: 100px;
        }
    }
</style>