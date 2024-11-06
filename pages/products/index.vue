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
  category: string
  price: string
  sku: number
  stock: number
  actions: string
}

const data = ref<Product[]>();

const columns: TableColumn<Product>[] = [{
  id: 'select',
  header: ({ table }) => h(UCheckbox, {
    'modelValue': table.getIsAllPageRowsSelected(),
    'indeterminate': table.getIsSomePageRowsSelected(),
    'onUpdate:modelValue': (value: boolean) => table.toggleAllPageRowsSelected(!!value),
    'ariaLabel': 'Select all'
  }),
  cell: ({ row }) => h(UCheckbox, {
    'modelValue': row.getIsSelected(),
    'onUpdate:modelValue': (value: boolean) => row.toggleSelected(!!value),
    'ariaLabel': 'Select row'
  }),
  enableSorting: false,
  enableHiding: false
}, {
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
}, {
  accessorKey: 'stock',
  header: 'Stock',
  cell: ({ row }) => {
    const stock = Number(row.getValue('stock'));
    let color;

    if (stock === 0) {
      color = 'error';
    } else if (stock > 0 && stock <= 10) {
      color = 'warning';
    } else if (stock > 10 && stock <= 50) {
      color = 'neutral';
    } else {
      color = 'success';
    }

    return h(UBadge, { class: 'capitalize', variant: 'solid', color }, () =>
      stock
    );
  }
}, {
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
const pages = [10,20,40,60,100,300];
const sortItems = ['Lowest Price', 'Highest Price', 'Lowest Stock', 'Top Sell']
</script>

<template>
<div class="py-4 flex items-center justify-between">
    <UBreadcrumb  :items="items" />
    <UButton to="/products/create">Add new Products</UButton>
</div>
  <div class="flex-1 divide-y divide-[var(--ui-border-accented)] w-full border border-gray-200 dark:border-gray-700 rounded-lg shadow-lg bg-white/60 dark:bg-gray-900/80">
    <div class="flex items-center justify-between p-3">
      <UInput
        :model-value="(table?.tableApi?.getColumn('email')?.getFilterValue() as string)"
        class="w-full max-w-sm"
        placeholder="Product/Bar Code/SKU"
        icon="i-heroicons-magnifying-glass  "
        @update:model-value="table?.tableApi?.getColumn('email')?.setFilterValue($event)"
      />
      <div class="flex item-center gap-1">
        <USelect placeholder="Category" :items="sortItems" class="w-40" />
        <USelect placeholder="Brand" :items="sortItems" class="w-40" />
      </div>
      <div class="flex items-center gap-1">
        <USelect placeholder="Sort By" :items="sortItems" class="w-36" />
        <USelect placeholder="Row" :items="pages" class="w-24" />
        <UDropdownMenu
        :items="table?.tableApi?.getAllColumns().filter(column => column.getCanHide()).map(column => ({
          label: upperFirst(column.id),
          type: 'checkbox' as const,
          checked: column.getIsVisible(),
          onUpdateChecked(checked: boolean) {
            table?.tableApi?.getColumn(column.id)?.toggleVisibility(!!checked)
          },
          onSelect(e?: Event) {
            e?.preventDefault()
          }
        }))"
        :content="{ align: 'end' }"
      >
        <UButton
          label="Columns"
          color="neutral"
          variant="outline"
          trailing-icon="i-heroicons-chevron-down-20-solid"
          class="ml-auto"
        />
      </UDropdownMenu>
      </div>
    </div>
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

    <div class="px-4 py-3.5 text-sm text-[var(--ui-text-muted)]">
      {{ table?.tableApi?.getFilteredSelectedRowModel().rows.length || 0 }} of
      {{ table?.tableApi?.getFilteredRowModel().rows.length || 0 }} row(s) selected.
    </div>
    <div class="flex justify-center p-3">
        <UPagination v-model:page="page" :total="100" />
    </div>
  </div>
</template>