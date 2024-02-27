<template>
  <Header/>
  <div class="container">
    <Balance :total="total"/>
    <IncomeExpenses :income="income" :expenses="expenses"/>
    <TransactionList @transaction-deleted="handleTransactionDeletion" :transactions="transactions"/>
    <AddTransaction @transaction-submitted="handleTransactionSubmission"/>
  </div>
</template>
<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import {onMounted, ref, watch} from "vue";
import {computed, defineProps} from "vue";
import {useToast} from "vue-toast-notification";

const transactions = ref([]);
const $toast = useToast({position: "top-right"});
const props = defineProps(["transactions"]);

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'))
  if (savedTransactions) {
    transactions.value = savedTransactions
  }
})

const income = computed(() => {
  return transactions.value
      .filter((transaction) => transaction.amount > 0)
      .reduce((acc, curr) => acc + curr.amount, 0)
      .toFixed(2);
});

const expenses = computed(() => {
  return transactions.value
      .filter((transaction) => transaction.amount < 0)
      .reduce((acc, curr) => acc + curr.amount, 0)
      .toFixed(2);
});

const total = computed(() => {
  return (+income.value + +expenses.value).toFixed(2);
});

const handleTransactionSubmission = (data) => {
  transactions.value.push({
    id: Math.floor(Math.random() * 1000000000),
    text: data.text,
    amount: data.amount,
  });
};

const handleTransactionDeletion = (id) => {
  transactions.value = transactions.value.filter(x => x.id !== id)
  $toast.success("transaction deleted");
}

watch(() => transactions, (value, oldValue, onCleanup) => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}, {deep: true})
</script>
