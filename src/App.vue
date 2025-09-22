<template>
  <div>
    <Header />

    <div class="container">
      <Balance :total="+total"/>
    </div>

    <IncomeExpense :income="+income"  :expenses="+expenses"/>


    <TransactionList :transactions="transactions"  @TransactionDeleted="handleTransactionDeleted"/>


    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
    
  </div>
</template>

<script setup>

import AddTransaction from './components/Expense_Tracker_vues/AddTransaction.vue';
import Balance from './components/Expense_Tracker_vues/Balance.vue';
import Header from './components/Expense_Tracker_vues/Header.vue';
import IncomeExpense from './components/Expense_Tracker_vues/IncomeExpense.vue';
import TransactionList from './components/Expense_Tracker_vues/TransactionList.vue';

import {ref, computed, onMounted} from 'vue';
import { useToast } from 'vue-toastification';

const toast = useToast();

const transactions = ref([]);

onMounted(()=>{
  const savedTransacctions = JSON.parse(localStorage.getItem('transactions'))

  if(savedTransacctions){
    transactions.value = savedTransacctions; 
  }
})



  // get total
const total = computed(()=>{
  return transactions.value.reduce((a,transaction)=>{return a+transaction.amt;},0);
})

// get income
const income = computed(()=>{
  return transactions.value.filter((transaction)=>transaction.amt>0).reduce((a,transaction)=>{return a+transaction.amt;},0);
})

// get expenses
const expenses = computed(()=>{
  return transactions.value.filter((transaction)=>transaction.amt<0).reduce((a,transaction)=>{return a+transaction.amt;},0);
})


// Add transaction
const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    name: transactionData.name,
    amt: transactionData.amt
  });

  savedTransacctionsToLocalStorage();

  toast.success("Transaction Added")
}


// Delete transaction
const handleTransactionDeleted = (transactionName) => {
  transactions.value = transactions.value.filter((t) => t.name != transactionName);
  savedTransacctionsToLocalStorage();
}


// saved to localStorage
const savedTransacctionsToLocalStorage = () =>{
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}

</script>

<style>

</style>