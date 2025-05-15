<template>
  <div class="q-pa-md">
    <q-list bordered separator v-if="entries.length > 0">
      <q-slide-item
        v-for="entry in entries"
        :key="entry.id"
        @right="onEntrySlideRight($event, entry)"
        right-color="negative"
      >
        <template v-slot:right>
          <q-icon name="delete" />
        </template>
        <q-item>
          <q-item-section class="text-weight-bold" :class="useAmountColorClass(entry.amount)">
            {{ entry.name }}
          </q-item-section>

          <q-item-section side class="text-weight-bold" :class="useAmountColorClass(entry.amount)">
            {{ useCurrencify(entry.amount) }}
          </q-item-section>
        </q-item>
      </q-slide-item>
    </q-list>
    <div v-else>
      <q-banner class="bg-blue-1 rounded-borders">
        <template v-slot:avatar>
          <q-icon name="info" color="blue-9" />
        </template>
        You have no entries.
      </q-banner>
    </div>
  </div>
  <q-footer class="bg-transparent">
    <div class="row q-mb-sm q-px-md q-py-md shadow-up-3 text-grey-6 text-h5">
      <div class="col">Balance</div>
      <div :class="useAmountColorClass(balance)" class="col text-right">
        {{ useCurrencify(balance) }}
      </div>
    </div>
    <q-form @submit="addEntry" class="row q-pa-sm q-col-gutter-md bg-primary">
      <div class="col">
        <q-input
          outlined
          bg-color="white"
          v-model="addEntryForm.name"
          placeholder="Insert the entry name"
          ref="nameRef"
          dense
        />
      </div>
      <div class="col">
        <q-input
          outlined
          bg-color="white"
          v-model.number="addEntryForm.amount"
          placeholder="Insert the amount"
          input-class="text-right"
          type="number"
          step="0.01"
          dense
        />
      </div>
      <div class="col col-auto"><q-btn type="submit" round color="secondary" icon="add" /></div>
    </q-form>
  </q-footer>
</template>

<script setup>
import { ref, computed, reactive } from 'vue'
import { uid, useQuasar } from 'quasar'
import { useCurrencify } from 'src/use/useCurrency.js'
import { useAmountColorClass } from 'src/use/useAmountColorClass.js'

const $q = useQuasar()

const entries = ref([])

const balance = computed(() => {
  return entries.value.reduce((accumulator, { amount }) => {
    return accumulator + amount
  }, 0)
})

const nameRef = ref(null)
const addEntryFormDefault = {
  name: '',
  amount: null,
}

const addEntryForm = reactive({
  ...addEntryFormDefault,
})

const addEntryFormReset = () => {
  Object.assign(addEntryForm, addEntryFormDefault)
  nameRef.value.focus()
}

const addEntry = () => {
  // const newEntry = {
  //   id: uid(),
  //   name: addEntryForm.name,
  //   amount: addEntryForm.amount,
  // }
  const newEntry = Object.assign({}, addEntryForm, { id: uid() })
  entries.value.push(newEntry)
  addEntryFormReset()
}

const onEntrySlideRight = ({ reset }, entry) => {
  $q.dialog({
    title: 'Delete Entry',
    message: `
    Delete this entry? 
    <div class='q-mt-sm text-weight-bold text-subtitle2 ${useAmountColorClass(entry.amount)}'>
      ${entry.name} : ${useCurrencify(entry.amount)}
      </div>
    `,
    html: true,
    cancel: true,
    persistent: true,
  })
    .onOk(() => {
      deleteEntry(entry.id)
    })
    .onCancel(() => {
      reset()
    })
}

const deleteEntry = (entryId) => {
  const index = entries.value.findIndex((entry) => entry.id === entryId)
  entries.value.splice(index, 1)
  $q.notify({
    message: 'Entry deleted successfully.',
    position: 'top',
    color: 'green',
  })
}
</script>
