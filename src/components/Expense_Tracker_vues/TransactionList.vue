<template>
  <div>
    <h3>History</h3>
    <ul id="list" class="list">

        <li v-for="x in transactions" :key="x.name" :class="x.amt<0 ? 'minus' : 'plus'"> 
          {{ x.name }} <span> ${{ x.amt }} </span> <button class="delete-btn" @click="deleteTransaction(x.name)">X</button>
        </li>
    </ul>
  </div>
</template>


<script setup>

import { defineProps } from 'vue';

const emit = defineEmits(['TransactionDeleted'])

const props = defineProps({
  transactions:{
    type: Array,
    required: true,
  }
})

const deleteTransaction = (name) =>{
  emit('TransactionDeleted', name)
}


</script>
