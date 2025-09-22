<template>
  <div>
    <h3>Add new Transaction</h3>
    <form id="form" @submit.prevent="onSubmit">
        <div class="form-control">
            <label for="text">Text</label>
            <input type="text" id="text" placeholder="Enter text..." v-model="name" />
        </div>
        <div class="form-control">
            <label for="amount">
                Amount <br />
                
                (negative - expense, positive - income)
            </label>
            <input type="number" id="amount" placeholder="Enter amount..." v-model="amt"/>
        </div>
        <button class="btn">Add Transaction</button>
    </form>
  </div>
</template>







<script setup>

import {ref} from 'vue';
import { useToast } from 'vue-toastification';  

const name = ref('');
const amt = ref('');

const emit = defineEmits(['transactionSubmitted']);

const toast = useToast();

const onSubmit = () =>{

    if(!name.value || !amt.value){
        toast.error("Both fileds must be filled");
        return;
    }

    console.log(name.value)
    console.log(amt.value)



    const transactionData = {
        name: name.value,
        amt: parseFloat(amt.value)
    }

    emit("transactionSubmitted", transactionData);

}
</script>