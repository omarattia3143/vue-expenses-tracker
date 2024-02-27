<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input
        v-model="text"
        type="text"
        name="text"
        id="text"
        placeholder="Enter text..."
      />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount
        <br />
        (negative - expense, positive - income)
      </label>
      <input
        v-model="amount"
        type="number"
        name="number"
        id="amount"
        placeholder="Enter amount..."
      />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>
<script setup>
import { useToast } from "vue-toast-notification";
import "vue-toast-notification/dist/theme-sugar.css";
import {ref} from "vue";
const $toast = useToast({ position: "top-right" });
const emit = defineEmits(["transactionSubmitted"]);
const text = ref("");
const amount = ref("");
const onSubmit = () => {
  if (!text.value || !amount.value) {
    $toast.error("all fields have to be filled");
    return;
  }

  $toast.success("form submitted");
  const formSubmissionData = {
    text: text.value,
    amount: parseFloat(amount.value),
  };

  emit("transactionSubmitted", formSubmissionData);
  text.value = "";
  amount.value = "";
};
</script>
