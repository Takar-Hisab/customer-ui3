<script setup lang="ts">
import { h, resolveComponent } from 'vue'
import { upperFirst } from 'scule'
import type { TableColumn } from '@nuxt/ui'
const UButton = resolveComponent('UButton')
const UCheckbox = resolveComponent('UCheckbox')
const UBadge = resolveComponent('UBadge')
const UDropdownMenu = resolveComponent('UDropdownMenu')

const {data: products} = useFetch('https://dummyjson.com/products');

const toast = useToast()

type Product = {
  thumbnail: string
  title: string
  price: string
  sku: number
}

const columns: TableColumn<Product>[] = [ 
  {
  accessorKey: 'thumbnail',
  header: 'Image',
  cell: ({ row }) => {
      const thumbnailUrl = row.getValue('thumbnail');
      return h('div', { class: 'text-center' }, [
        h('img', {
          src: thumbnailUrl,
          alt: 'Product Thumbnail',
          class: 'w-12 h-12 object-contain' ,
        })
      ]);
    }
}, 
{
  accessorKey: 'title',
  header: 'Title',
  cell: ({ row }) => h('div', { class: 'max-w-xs truncate' }, row.getValue('title'))
}, 
{
  accessorKey: 'category',
  header: 'Category'
}, {
  accessorKey: 'price',
    header: 'Price',
}, {
  accessorKey: 'sku',
  header: 'SKU' 
},
   {
  id: 'actions',
  enableHiding: false,
  cell: ({ row }) => {
    const items = [{
      type: 'label',
      label: 'Actions'
    }, {
      label: 'Copy payment ID',
      onSelect() {
        navigator.clipboard.writeText(row.original.id)

        toast.add({
          title: 'Payment ID copied to clipboard!',
          color: 'success',
          icon: 'i-heroicons-check-circle'
        })
      }
    }, {
      label: row.getIsExpanded() ? 'Collapse' : 'Expand',
      onSelect() {
        row.toggleExpanded()
      }
    }, {
      type: 'separator'
    }, {
      label: 'View customer'
    }, {
      label: 'View payment details'
    }]

    return h('div', { class: 'text-right' }, h(UDropdownMenu, {
      content: {
        align: 'end'
      },
      items
    }, () => h(UButton, {
      icon: 'i-heroicons-ellipsis-vertical-20-solid',
      color: 'neutral',
      variant: 'ghost',
      class: 'ml-auto'
    })))
  }
}]

const table = useTemplateRef('table')

const page = ref(5);

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
        label: 'All Products',
        icon: 'i-heroicons-link',
    },
]);
</script>

<template>
  <div class=" w-full border border-gray-200 dark:border-gray-700 rounded-lg shadow-lg bg-white/60 dark:bg-gray-900/80">
    <h3 class="p-4">Recent Orders</h3>
    <UTable
      ref="table"
      :data="products?.products"
      :columns="columns"

      :ui="{td:'text-black dark:text-gray-300 font-semibold', th:'border-0'}"
      sticky
      class="h-[70vh]"
    >
      <template #expanded="{ row }">
        <pre>{{ row.original }}</pre>
      </template>
    </UTable>
  </div>
</template>