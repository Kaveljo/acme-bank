<template>
    <div class="home">
        <ActionsDialog v-if="toggleDialog" @close="closeDialog" :details="accountDetails" :action="currentAction"></ActionsDialog>
        <HistoryDialog v-if="toggleHistory" @close="closeDialog" :history="history"></HistoryDialog>
        <table class="info-table">
            <tr>
                <th></th>
                <th>TYPE</th>
                <th>ID</th>
                <th>BALANCE</th>
                <th>ACTION</th>
            </tr>
            <tr v-for="account in accounts" :key="account.id">
                <td class="view" @click="showHistory(account.history)"><TableButton name="VIEW"></TableButton></td>
                <td>{{account.type}}</td>
                <td>{{account.id}}</td>
                <td>R{{account.balance.toFixed(2)}}</td>
                <td class="actions"><TableButton name="WITHDRAW" @click="updateData('WITHDRAW',account)"></TableButton><TableButton name="DEPOSIT" @click="updateData('DEPOSIT',account)"></TableButton></td>
            </tr>
        </table>
    </div>
</template>

<script>
import TableButton from '../components/UI/TableButton.vue'
import ActionsDialog from '../components/layout/ActionsDialog.vue'
import HistoryDialog from '../components/layout/HistoryDialog.vue'

export default {
    components:{
        TableButton,
        ActionsDialog,
        HistoryDialog
    },

    data(){
        return{
            accounts:[],
            accountDetails: null,
            currentAction: '',
            history:[],
            toggleDialog: false,
            toggleHistory: false
        }
    },
    methods:{
        updateData(action,account){
            this.currentAction = action;
            this.accountDetails = account;
            this.toggleDialog = true;
        },
        showHistory(data){
            this.history = data;
            this.toggleHistory = true;
        },
        closeDialog(){
            this.toggleDialog = false;
            this.toggleHistory = false;
        }
    },
    created(){
        fetch('http://localhost:3000/accounts')
        .then(response => {
            return response.json();
        })
        .then(data => {
            this.accounts = data;
        })
    }
}
</script>

<style scoped>
.info-table{
    width: 100%;
    border-collapse: collapse;
}
th,td{
    padding: 10px 5px;
    box-sizing: border-box;
    border:1px solid #000;
    text-align: center;
}

.view,
.actions{
    width: 1%;
    white-space: nowrap;
}

.actions > button{
    min-width: 100px;
}

@media screen and (max-width: 900px){
    .home{
        overflow-x: scroll;
    }
    .info-table{
    width: 90%;
    margin: 0 auto;
    border-collapse: collapse;
}
}
</style>