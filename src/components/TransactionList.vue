<template>
    <h3>History</h3>
        <ul id="list" class="list">
            <li
            v-if="transactions.length > 0" 
            v-for="transaction in transactions" 
            :key="transaction.id" 
            :class="transaction.amount < 0 ? 'minus' : 'plus'">
                {{ transaction.text }} <span>P{{ transaction.amount }}</span>
                <button class="delete-btn" @click="removeTransaction(transaction.id)">x</button>
            </li>
            <li v-else>
                No transactions found
            </li>
            <!-- <li class="minus">
                Cash <span>-P400</span><button class="delete-btn">x</button>
            </li>
            <li class="plus">
                Salary <span>+P400</span><button class="delete-btn">x</button>
            </li> -->
        </ul>
</template>

<!-- <script>
    // 1.) Options api way
    // export default {
    //     data:() => ({
    //         transactions: [
    //             { id: 1, text: 'Flower', amount: -19.99 },
    //             { id: 2, text: 'Salary', amount: +219.97 },
    //             { id: 3, text: 'Book', amount: -10 },
    //             { id: 4, text: 'Video Games', amount: 160 },
    //         ]
    //     })
    // }

    // 2. Compositions API way (explicit setup) - version 3.2
    export default {
        setup() {
            const transactions = [
                { id: 1, text: 'Flower', amount: -19.99 },
                { id: 2, text: 'Salary', amount: +219.97 },
                { id: 3, text: 'Book', amount: -10 },
                { id: 4, text: 'Video Games', amount: 160 },
            ];

            // myFunction() {}

            return {
                transactions,
                // myFunction
            }
        }
    }
</script> -->

<!-- New Way -->
<!-- <script setup>
    const transactions = [
        { id: 1, text: 'Flower', amount: -19.99 },
        { id: 2, text: 'Salary', amount: +219.97 },
        { id: 3, text: 'Book', amount: -10 },
        { id: 4, text: 'Video Games', amount: 160 },
    ];
</script> -->

<script setup>
import { defineProps } from 'vue';

const props = defineProps({
    transactions: {
        type: Array,
        required: true
    }
});

const emit = defineEmits(['transactionDeleted']);

const removeTransaction = (id) => {
    emit('transactionDeleted', id);
}
</script>