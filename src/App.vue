<template>
    <Header></Header>
    <div class="container">
        <Balance :total="+total"></Balance>
        <IncomeExpenses :income="+income" :expenses="+expenses"></IncomeExpenses>
        <TransactionList @transactionDeleted="removeTransaction" :transactions="transactions"></TransactionList>
        <AddTransaction @transactionSubmit="handleTransaction"></AddTransaction>
    </div>
</template>

<!-- Old Way - Options API -->
<!-- <script>
    import Header from './components/Header.vue';
    import Balance from './components/Balance.vue';
    import IncomeExpenses from './components/IncomeExpenses.vue';
    import TransactionList from './components/TransactionList.vue';
    import AddTransaction from './components/AddTransaction.vue';

    export default {
        components: {
            Header,
            Balance,
            TransactionList,
            AddTransaction
        }
    }
</script> -->

<!-- New Way - Compositions API -->
<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';

import { ref, computed, onMounted } from 'vue';
import {useToast} from 'vue-toastification';

// Defining the toast
const toast = useToast();

// Defining the transaction in global state
const transactions = ref([]);

onMounted(() => {
    const savedTransactions = JSON.parse(localStorage.getItem("transactions"));

    if (savedTransactions) {
        transactions.value = savedTransactions;
    }
})


// Computed properties
const total = computed(() => transactions.value.reduce((accumulator, transaction) => accumulator + transaction.amount, 0));

// Get income
const income = computed(() => transactions.value.filter((transaction) => transaction.amount > 0).reduce((accumulator, transaction) => accumulator + transaction.amount, 0).toFixed(2));

// Get expenses
const expenses = computed(() => {
    return transactions.value
    .filter(transaction => transaction.amount < 0)
    .reduce((accumulator, transaction) => accumulator + transaction.amount, 0);
});

// Add Transaction
const handleTransaction = (transactionData) => {
    // Add it to the transactions list
    transactions.value.push({
        id: generateUniqueId(),
        text: transactionData.text,
        amount: transactionData.amount
    });
    saveTransactionsToLocalStorage();
    toast.success('Transaction added');
};

// Generate unique ID
const generateUniqueId = () => Math.floor(Math.random() * 1000000);

// Remove Transaction
const removeTransaction = (id) => {
    // remove the index from transactions list
    // const index = transactions.value.findIndex(transaction => id === transaction.id);
    // transactions.value.splice(index, 1);

    transactions.value = transactions.value.filter(transaction => transaction.id !== id);

    saveTransactionsToLocalStorage();
    toast.success('Transaction removed');
};

// Save to local storage
const saveTransactionsToLocalStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value))
}
</script>