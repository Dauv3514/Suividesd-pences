<template>
    <h3>Dépenses</h3>
    <ul id="list" class="list">
        <li v-for="transaction in transactions" 
            :key="transaction.id" 
            :class="transaction.amount < 0 ? 'minus' : 'plus'">
            {{ transaction.text }} 
            <span>-${{ transaction.amount }}
            </span>
            <button @click="deleteTransaction(transaction.id)" class="delete-btn">x</button>
        </li>
    </ul>
    <!-- <ul id="list" class="list">
        <li class="minus">
            Cash <span>-$400</span>
            <button class="delete-btn">x</button>
        </li>
        <li class="plus">
            Paycheck <span>-$800</span>
            <button class="delete-btn">x</button>
        </li>
    </ul> -->
</template>

// export default {
//     data() {
//         return {
//             transactions: [
//                 {id:1, text: 'Fleurs', amount: 19.99 },
//                 {id:1, text: 'Bijou', amount: -22.23 },
//                 {id:1, text: 'Livres', amount: 29.39 },
//                 {id:1, text: 'Chaussures', amount: -49.99 },
//             ]
//         }
//     }
//     setup() {
//         const transactions = [
//             {id:1, text: 'Fleurs', amount: 19.99 },
//             {id:1, text: 'Bijou', amount: -22.23 },
//             {id:1, text: 'Livres', amount: 29.39 },
//             {id:1, text: 'Chaussures', amount: -49.99 },
//         ];
//         return {
//             transactions,
//         }
//     }
// }

<script setup>
    import { defineProps } from 'vue';
    const emit = defineEmits(['transactionDeleted']);
    const props = defineProps({
        transactions:{
            type: Array,
            required: true
        }
    });
    const deleteTransaction = (id) => {
        emit('transactionDeleted', id);
    }
</script>