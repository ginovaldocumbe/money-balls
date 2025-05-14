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
</template>

<script setup>
import { ref } from 'vue'
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
</script>
