<template>
    <teleport to="body">
        <div class="card">
            <div class="card-header">
                <h3>AMOUNT</h3>
                <button @click="closeDialog">X</button>
            </div>
            <div class="card-content">
                <div class="card-input">
                    <input type="number" name="amount" id="amount" min="0" v-model="amount">
                </div>
                <TableButton :name="action" @click="updateBalance(action, amount)"></TableButton>
            </div>
        </div>
        <div class="actionDialog" @click="closeDialog"></div>
    </teleport>
</template>

<script>
import TableButton from '../UI/TableButton.vue'
export default {
    props:['action', 'details'],
    emits: ['close'],
    components:{
        TableButton
    },
    data(){
        return{
            amount:0
        }
    },
    methods:{
        closeDialog(){
            this.$emit('close');
        },
        
        updateBalance(action, amount){
            action === "WITHDRAW" ? this.withdrawAction(amount) : this.depositAction(amount)
        },
        
        withdrawAction(amount){
            if(!this.authWithdraw(amount)){
                alert("You have insufficient funds!")
                this.closeDialog();
                return;
            }

            this.details.balance -= parseInt(amount)
            this.addTransaction(amount);

            fetch('http://localhost:3000/accounts/'+this.details.id, {
            method: 'PUT',
            body: JSON.stringify({
                    type: this.details.type,
                    id: this.details.id,
                    balance: this.details.balance,
                    history: this.details.history,
                    max_overdraft: this.details.max_overdraft
                }),
                headers: {"Content-Type": "application/json"}
            })
            .then(res => res.json())
            .then(data => console.log(data))
            .catch(err => console.log(err));
            this.closeDialog();
        },

        depositAction(amount){
            this.details.balance += parseInt(amount);
            this.addTransaction(amount);

            fetch('http://localhost:3000/accounts/'+this.details.id, {
            method: 'PUT',
            body: JSON.stringify({
                    type: this.details.type,
                    id: this.details.id,
                    balance: this.details.balance,
                    history: this.details.history,
                    max_overdraft: this.details.max_overdraft
                }),
                headers: {"Content-Type": "application/json"}
            })
            .then(res => res.json())
            .then(data => console.log(data))
            .catch(err => console.log(err));
            this.closeDialog();
        },

        authWithdraw(amount){
            if(this.details.type === "CURRENT"){
                return this.details.balance - parseInt(amount)  <  -this.details.max_overdraft ? 0 : 1;
            }else{
                return 1000 > this.details.balance - parseInt(amount) ? 0 : 1;
            }
        },

        addTransaction(amount){
            var today = new Date();
            var dd = today.getDate();
            var mm = today.getMonth()+1; 
            var yyyy = today.getFullYear();

            today = dd+'-'+mm+'-'+yyyy;
            const newTransaction = {
                balance: this.details.balance,
                amount: parseInt(this.amount *= this.action === "WITHDRAW" ? -1 : 1),
                date: today
            }
            
            this.details.history.push(newTransaction)
        }
    }
}
</script>

<style scoped>
    .actionDialog{
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
    .card-input{
        margin: 10px auto;
        max-width: 80%;
    }
    input{
        width: 100%;
        padding: 10px;
        border-radius: 5px;
        
        box-sizing: border-box;
    }
    @media screen and (max-width: 900px){
        .home{
            overflow-x: scroll;
        }
    }
</style>