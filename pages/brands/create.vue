<script setup lang="ts">
import { z } from 'zod'
import type { FormSubmitEvent } from '#ui/types'
const titleMaxLength = 255;
const schema = z.object({
  name: z.string().max(255, 'Must be at most 255 characters'),
  price: z.number(),
  image: z.string(),
});

type Schema = z.output<typeof schema>

const state = reactive<Partial<Schema>>({
  name: undefined,
  price: undefined,
  image: undefined,
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
        label: 'Products',
        icon: 'i-heroicons-cube',
    },
    {
        label: 'Add a new Brand',
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
        <UFormField label="Brand Name" name="name" class="w-full">
        <UInput v-model="state.name" variant="subtle" size="xl" :maxlength="titleMaxLength" class="w-full">
            <template #trailing>
                <div
                    highlight
                    id="character-count"
                    class="text-xs text-[var(--ui-text-muted)] tabular-nums"
                    aria-live="polite"
                    role="status"
                >
                    {{ state?.name?.length ?? 0 }}/{{ titleMaxLength  }}
                </div>
                </template>
        </UInput>
        </UFormField>

        
        <UFormField label="Brand Logo" name="image" class="w-full">
            <UInput type="file" variant="subtle" size="xl" />
        </UFormField>
        

        <div class="flex items-center space-x-2">
            <UButton type="reset" to="/brands">Cancel</UButton>
            <UButton type="submit">Save Brand</UButton>
        </div>
    </UForm>
  </div>
</template>

