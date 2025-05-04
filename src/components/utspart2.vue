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

function filteredExpenses(day) {
  const dayExpenses = expenses.value[day];
  return filter.value === 'uncompleted' 
    ? dayExpenses.filter(exp => !exp.completed)
    : dayExpenses;
}

function calculateDayTotal(day) {
  return expenses.value[day].reduce((total, exp) => total + exp.amount, 0);
}

function calculateWeekTotal() {
  return days.reduce((total, day) => total + calculateDayTotal(day), 0);
}

function saveToLocalStorage() {
  localStorage.setItem('kosExpenses', JSON.stringify(expenses.value));
}

function loadFromLocalStorage() {
  const saved = localStorage.getItem('kosExpenses');
  if (saved) {
    expenses.value = JSON.parse(saved);
  }
}

onMounted(() => {
  loadFromLocalStorage();
});
</script>

<template>
    <div class="expense-tracker">
    <h1>Pengeluaran Anak Kos</h1>
    
    <div class="controls">
      <select v-model="selectedDay">
        <option v-for="day in days" :value="day">{{ day }}</option>
      </select>
      <input 
        v-model="newExpense.text" 
        placeholder="Misal: Makan siang..."
        @keyup.enter="addExpense"
      >
      <input
        v-model.number="newExpense.amount"
        type="number"
        placeholder="Rp"
      >
      <button @click="addExpense">Tambah</button>
    </div>
    </div>

</template>

<style>

</style>