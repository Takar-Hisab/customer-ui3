<script setup lang="ts">
import { z } from 'zod'
import type { FormSubmitEvent } from '#ui/types'
const schema = z.object({
  name: z.string().max(255, 'Must be at most 255 characters'),
  price: z.number()
});

type Schema = z.output<typeof schema>

const state = reactive<Partial<Schema>>({
  name: undefined,
  price: undefined,
})

const toast = useToast()
async function onSubmit(event: FormSubmitEvent<Schema>) {
  toast.add({ title: 'Success', description: 'The form has been submitted.', color: 'success' })
  console.log(event.data)
}

const items = ref([
    {
        label: 'Dashbard',
        icon: 'i-heroicons-home',
        to: '/dashboard'
    },
    {
        label: 'Purchases',
        icon: 'i-heroicons-cube',
        to:'/purchases'
    },
    {
        label: 'Add Purchase',
        icon: 'i-heroicons-link',
    },
]);
</script>

<template>
    <div class="py-4">
        <UBreadcrumb  :items="items" />
    </div>
  <div class="border border-gray-200 dark:border-gray-700 rounded-lg shadow-lg bg-white/60 dark:bg-gray-900/80 max-w-xl mx-auto p-4">
    <UForm :schema="schema" :state="state" class="space-y-4" @submit="onSubmit">
        <UFormField label="Supplier" name="name" class="w-full">
        <UInput v-model="state.name" variant="subtle" size="xl" class="w-full" />
        </UFormField>

        <div class="flex -mx-2">
            <UFormField label="Reference No" name="price" class="w-1/2 px-2">
                <UInput v-model="state.price" type="number" class="w-full" variant="subtle" size="xl" />
            </UFormField>
            <UFormField label="Purchase Date" name="category_id" class="w-1/2 px-2">
                <UInput v-model="state.price" type="date" class="w-full" variant="subtle" size="xl" />
            </UFormField>
        </div>

        <div class="flex -mx-2">
            <UFormField label="Grand Total" name="price" class="w-1/2 px-2">
            <UInput v-model="state.price" type="number" class="w-full" variant="subtle" size="xl" />
            </UFormField>
            <UFormField label="Payment Status" name="category_id" class="w-1/2 px-2">
                <USelect v-model="value" :items="items" class="w-full" variant="subtle" size="xl" />
            </UFormField>
        </div>

        <div class="flex -mx-2">
            <UFormField label="Purchase Status" name="status" class="w-1/2 px-2">
                <USelect v-model="value" :items="items" class="w-full" variant="subtle" size="xl" />
            </UFormField>
            <UFormField label="Purchase Tax" name="category_id" class="w-1/2 px-2">
                <USelect v-model="value" :items="items" class="w-full" variant="subtle" size="xl" />
            </UFormField>
        </div>
        <UFormField label="Additional Notes" name="image" class="w-full">
            <UTextarea placeholder="Type something..." class="w-full" />
        </UFormField>
        <div class="flex items-center space-x-2">
            <UButton type="reset" to="/purchases">Cancel</UButton>
            <UButton type="submit">Save Purchase</UButton>
        </div>
    </UForm>
  </div>
</template>

