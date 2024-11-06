<script setup lang=ts>

import { z } from 'zod'
import type { FormSubmitEvent } from '#ui/types'
const titleMaxLength = 255;
const schema = z.object({
  name: z.string().max(255, 'Must be at most 255 characters'),
  price: z.number()
});

type Schema = z.output<typeof schema>

const state = reactive<Partial<Schema>>({
  name: undefined,
  price: undefined,
});
const items = ref([
    {
        label: 'Dashbard',
        icon: 'i-heroicons-home',
        to: '/dashboard'
    },
    {
        label: 'Settings',
        icon: 'i-heroicons-link',
    },
]);
</script>

<template>
<div class="py-4">
    <UBreadcrumb  :items="items" />
</div>
<div class="w-full max-w-xl mx-auto border border-gray-200 dark:border-gray-700 rounded-lg shadow-lg bg-white/60 dark:bg-gray-900/80 p-4">
    <UForm :schema="schema" :state="state" class="space-y-4" @submit="onSubmit">
        <UFormField label="Shop Name" name="name" class="w-full">
        <UInput v-model="state.name" variant="subtle" size="xl" :maxlength="titleMaxLength" class="w-full" />
        </UFormField>
        
        <UFormField label="Shop Logo" name="image" class="w-full">
            <UInput type="file" variant="subtle" size="xl" />
        </UFormField>
        
        <UButton type="submit">
        Save Changes
        </UButton>
    </UForm>
</div>
</template>