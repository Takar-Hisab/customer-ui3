<script setup lang="ts">
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
        label: 'Add a new product',
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
        <UFormField label="Product Name" name="name" class="w-full">
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

        <div class="flex -mx-2">
            <UFormField label="Product Price" name="price" class="w-1/3 px-2">
            <UInput v-model="state.price" type="number" class="w-full" variant="subtle" size="xl" />
            </UFormField>
            <UFormField label="Product Category" name="category_id" class="w-1/3 px-2">
                <USelect v-model="value" :items="items" class="w-full" variant="subtle" size="xl" />
            </UFormField>
            <UFormField label="Product Brand" name="brand_id" class="w-1/3 px-2">
                <USelect v-model="value" :items="items" class="w-full" variant="subtle" size="xl" />
            </UFormField>
        </div>
        <div class="flex -mx-2">
            <UFormField label="Stock" name="stock" class="w-1/3 px-2">
            <UInput v-model="state.price" type="number" class="w-full" variant="subtle" size="xl" />
            </UFormField>
            <UFormField label="Product SKU" name="category_id" class="w-1/3 px-2">
                <UInput v-model="state.price" type="number" class="w-full" variant="subtle" size="xl" />
            </UFormField>
            <UFormField label="Barcode Type" name="brand_id" class="w-1/3 px-2">
                <USelect v-model="value" :items="items" class="w-full" variant="subtle" size="xl" />
            </UFormField>
        </div>
        <UFormField label="Product Image" name="image" class="w-full">
            <UInput type="file" variant="subtle" size="xl" />
        </UFormField>
        <div>
            <p class="text-sm text-gray-500 py-4">Tax Information</p>
            <div class="flex -mx-2">
                <UFormField label="Applicable Tax" name="category_id" class="w-1/3 px-2">
                    <UInput v-model="state.price" type="number" class="w-full" variant="subtle" size="xl" />
                </UFormField>
                <UFormField label="Selling Price Tax Type" name="brand_id" class="w-1/3 px-2">
                    <USelect v-model="value" :items="items" class="w-full" variant="subtle" size="xl" />
                </UFormField>
            </div>
        </div>

        <UButton type="submit">
        Save Product
        </UButton>
    </UForm>
  </div>
</template>

