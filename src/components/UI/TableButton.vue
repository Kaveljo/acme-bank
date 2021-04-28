<template>
    <button>{{name}}</button>
</template>
<script>
export default {
    props:['name']
}
</script>
<style scoped>
button{
    padding: 10px;
    margin: 0 10px;
    background-color: #DB0073;
    border: 1px solid #DB0073;
    color: #fff;
    transition: all 0.5s;
    font-weight: 700;
}
button:hover{
    background-color: #fff;
    color: #DB0073;
}
</style>