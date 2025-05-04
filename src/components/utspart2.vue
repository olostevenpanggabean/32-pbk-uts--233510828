<script setup>
import { ref, onMounted } from 'vue';

// function selected day
const days = ['Senin', 'Selasa', 'Rabu', 'Kamis', 'Jumat', 'Sabtu', 'Minggu'];
const selectedDay = ref('Minggu');
const filter = ref('all');
const expenses = ref({});
const newExpense = ref({
  text: '',
  amount: 0,
  completed: false
});

days.forEach(day => {
  expenses.value[day] = [];
});

function addExpense() {
  if (!newExpense.value.text || newExpense.value.amount <= 0) return;
  
  expenses.value[selectedDay.value].push({
    ...newExpense.value,
    id: Date.now()
  });
  
  newExpense.value = { text: '', amount: 0, completed: false };
  saveToLocalStorage();
}

function removeExpense(day, index) {
  expenses.value[day].splice(index, 1);
  saveToLocalStorage();
}
</script>

<template>

</template>

<style>

</style>