<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="transaction in transactions"
      :key="transaction.id"
      :class="transaction.amount < 0 ? 'minus' : 'plus'"
    >
      {{ transaction.text }} <span>${{ transaction.amount }}</span>
      <button @click="deleteTransaction(transaction.id)" class="delete-btn">x</button>
    </li>
  </ul>
</template>
<script setup>
import {computed} from "vue";
const emit = defineEmits(['transactionDeleted']);
const props = defineProps(["transactions"]);
const income = computed(() => {
  transactions.value
    .filter(() => transaction.amount > 0)
    .reduce((acc, curr) => acc + curr.amount, 0)
    .toFixed(2);
});

const expenses = computed(() => {
  transactions.value
    .filter(() => transaction.amount < 0)
    .reduce((acc, curr) => acc + curr.amount, 0)
    .toFixed(2);
});

const deleteTransaction = (id) =>{
  emit('transactionDeleted', id);
}
</script>
