<template>
  <div class="q-pa-md">
    <q-list bordered separator>
      <q-item v-for="entry in entries" :key="entry.id">
        <q-item-section class="text-weight-bold" :class="useAmountColorClass(entry.amount)">
          {{ entry.name }}
        </q-item-section>

        <q-item-section side class="text-weight-bold" :class="useAmountColorClass(entry.amount)">
          {{ useCurrencify(entry.amount) }}
        </q-item-section>
      </q-item>
    </q-list>
  </div>
  <q-footer class="bg-transparent">
    <div class="row q-mb-sm q-px-md q-py-md shadow-up-3 text-grey-6 text-h5">
      <div class="col">Balance</div>
      <div :class="useAmountColorClass(balance)" class="col text-right">
        {{ useCurrencify(balance) }}
      </div>
    </div>
    <div class="row q-pa-sm q-col-gutter-md bg-primary">
      <div class="col">
        <q-input outlined bg-color="white" v-model="text" placeholder="Insert your name" dense />
      </div>
      <div class="col">
        <q-input
          outlined
          bg-color="white"
          v-model="text"
          placeholder="Insert the amount"
          input-class="text-right"
          type="number"
          step="0.01"
          dense
        />
      </div>
      <div class="col col-auto"><q-btn round color="secondary" icon="add" /></div>
    </div>
  </q-footer>
</template>

<script setup>
import { ref, computed } from 'vue'
import { useCurrencify } from 'src/use/useCurrency.js'
import { useAmountColorClass } from 'src/use/useAmountColorClass.js'

const entries = ref([
  {
    id: 'id1',
    name: 'Teste1',
    amount: 2500,
  },
  {
    id: 'id2',
    name: 'Teste2',
    amount: -500,
  },
  {
    id: 'id3',
    name: 'Teste3',
    amount: -100,
  },
])

const balance = computed(() => {
  return entries.value.reduce((accumulator, { amount }) => {
    return accumulator + amount
  }, 0)
})
</script>
