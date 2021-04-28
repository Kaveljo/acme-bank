<template>
    <teleport to="body">
        <div class="card">
            <div class="card-header">
                <h3>HISTORY</h3>
                <button @click="closeDialog">X</button>
            </div>
            <div class="card-content">
                <table>
                    <tr class="history-header">
                        <th>BALANCE</th>
                        <th>AMOUNT</th>
                        <th>DATE</th>
                    </tr>
                    <tr v-for="(item,i) in history" :key="i" class="history-data">
                        <td>{{item.balance}}</td>
                        <td>{{item.amount}}</td>
                        <td>{{item.date}}</td>
                    </tr>
                </table>
                <TableButton :name="'BACK'" class="dialogButton" @click="closeDialog"></TableButton>
            </div>
        </div>
        <div class="historyDialog" @click="closeDialog"></div>
    </teleport>
</template>

<script>
import TableButton from '../UI/TableButton.vue'
export default {
    props:['history'],
    emits: ['close'],

    components:{
        TableButton
    },
    methods:{
        closeDialog(){
            this.$emit('close');
        },
    }
}
</script>

<style scoped>
.historyDialog{
    position: fixed;
    top: 0;
    left: 0;
    height: 100vh;
    width: 100%;
    background-color: rgba(0, 0, 0, 0.75);
}
.card{
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 50%;
    border:1px solid #000;
    border-radius: 5px;
    z-index: 10;
    max-height: 90vh;
    overflow-y: scroll;
}
.card-header{
    background-color: #DB0073;
    color: #fff;
    padding: 10px;
    display: flex;
    justify-content: space-between;
}

.card-header > button{
    background: none;
    color:#fff;
    border: 1px solid #fff;
    font-weight: bold;
}
.card-content{
    background-color: #fff;
    padding: 10px;
    text-align: center;
}

table{
    width: 100%;
    border-collapse: collapse;
    margin: 10px 0;
}
th,td{
    padding: 10px 5px;
    box-sizing: border-box;
    border:1px solid #000;
    text-align: center;
}
@media screen and (max-width: 900px){
    .card{
        width: 100%;
        padding: 0 10px;
        box-sizing: border-box;
    }
}
</style>