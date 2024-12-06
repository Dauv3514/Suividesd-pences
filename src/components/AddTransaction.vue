<template>
    <h3> Ajouter des transactions</h3>
    <form id="form"@submit.prevent="onSubmit">
        <div class="form-control">
            <label for="text"> Texte</label>
            <input type="text" id="text" v-model="text" placeholder="Entre le texte..."/>
        </div>
        <div class="form-control">
            <label for="amount"> Montant <br /> (negatives - Charges, positives - Revenus)</label>
            <input type="amount" id="amount" v-model="amount" placeholder="Entrer le montant..."/>
        </div>
        <button class="btn">Ajouter une transaction</button>
    </form>
</template>

<script setup>

import {ref} from 'vue';
import{useToast} from 'vue-toastification';

const text = ref('');
const amount = ref('');
const emit = defineEmits(['transactionSubmitted'])
const toast = useToast();

const onSubmit = () => {
    if(!text.value || !amount.value) {
        toast.error('Tu as une erreur');
        return;
    }
    const transactionData = {
    text : text.value,
    amount: parseFloat(amount.value)
    }

    emit('transactionSubmitted', transactionData);

    text.value='';
    amount.value='';
}

</script>