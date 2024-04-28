<template>
    <div id="card">
        <p id="term">Term: {{ term }}</p>
        <p id="definition">Definition: {{ def }}</p>
    </div>
</template>

<script>
export default{
    props: ['term', 'def', 'sent'],
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
    @media only screen and (max-width: 700px){
        #card{
            width: 200px;
            height: 100px;
        }
    }
</style>