<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import AddTransactions from './components/AddTransactions.vue';
import TransactionList from './components/TransactionList.vue';

import { computed, ref } from 'vue';

const transactions = ref([])

const sum = computed(()=>{
    return transactions.value.reduce((acc, x)=>{
        return acc+x.amount
    },0)
})

const moneyIn = computed(()=>{
    return transactions.value
    .filter((x)=>x.amount>0)
    .reduce((acc, x)=>{
        return acc+x.amount
    },0)
})

const moneyOut = computed(()=>{
    return transactions.value
    .filter((x)=>x.amount<0)
    .reduce((acc, x)=>{
        return acc+x.amount
    },0)
})

const handelTransaction = (transactionData) => {
    transactions.value.push({
        text: transactionData.text,
        amount: transactionData.amount,
    })
    
}

const handelDelete =(id) => {
    transactions.value = transactions.value.filter((x)=> x.id !== id)
}

</script>


<template>
    <Header></Header>
    <div class="container">
        <Balance :total="sum"></Balance>
        <IncomeExpense :income="moneyIn" :expense="moneyOut"></IncomeExpense>
        <AddTransactions @transactionSubmitted="handelTransaction"></AddTransactions>
        <TransactionList :transactions="transactions" @transactionDeleted="handelDelete"></TransactionList>
    </div>

    
</template>