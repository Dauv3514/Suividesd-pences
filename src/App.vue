<template>
  <Header />
  <div class="container">
    <Balance :total="+total"/>
    <IncomeExpense :income="+income" :expenses="+expenses"/>
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted"/>
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted"/>
  </div>
</template>

<script setup>
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import IncomeExpense from './components/IncomeExpense.vue';
  import TransactionList from './components/TransactionList.vue';
  import AddTransaction from './components/AddTransaction.vue';
  import{useToast} from 'vue-toastification';

  import { ref, computed, onMounted} from 'vue';

  const toast = useToast();

  const transactions = ref([
      // {id:1, text: 'Fleurs', amount: 19.99 },
      // {id:2, text: 'Bijou', amount: -22.23 },
      // {id:3, text: 'Livres', amount: 29.39 },
      // {id:4, text: 'Chaussures', amount: -49.99 },
  ]);

  onMounted(() => {
    const savedTransactions = JSON.parse(localStorage.getItem('transactions'));
    if(savedTransactions) {
      transactions.value = savedTransactions
    }
  })

  // Get total

  const total = computed(() => {
    return transactions.value.reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0);
  })

  // Get Charges

  const income = computed(() => {
    return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0).toFixed(2);
  })

  // Get revenus 

  const expenses = computed(() => {
    return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0).toFixed(2);
  })

  // Add transaction

  const handleTransactionSubmitted = (transactionData) => {
    transactions.value.push({
      id: generateUniqueId(),
      text: transactionData.text,
      amount: transactionData.amount,
    });
    saveTransactionsToLocalStorage();
    toast.success('Transaction ajoutée');
  };

  // Generate unique Id
  const generateUniqueId = () => {
    return Math.floor(Math.random() * 1000000);
  };

  // Delete transaction

  const handleTransactionDeleted = (id) => {
    transactions.value = transactions.value.filter((transaction) => transaction.id !== id);
    saveTransactionsToLocalStorage();
    toast.success('Transaction deleted');
  };

  // Save to localstorage

  const saveTransactionsToLocalStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value));
  }

</script>