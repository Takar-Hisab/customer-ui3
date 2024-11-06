<script setup lang="ts">
const items = ref([
  [
    {
        label: 'Dashboard',
        icon: 'i-heroicons-home',
        to: '/dashboard'
    },
    {
      label: 'Products',
      icon: 'i-heroicons-cube',
      children: [
        {
          label: 'All Products',
          to:'/products',
          icon:'i-heroicons-arrow-down-right'
        },
        {
          label: 'Categories',
          to:'/categories',
          icon:'i-heroicons-arrow-down-right'
        },
        {
          label: 'Brands',
          to:'/brands',
          icon:'i-heroicons-arrow-down-right'
        },
      ]
    },
    {
      label: 'Service',
      icon: 'i-heroicons-cube',
      children: [
        {
          label: 'All Service',
          to:'/service',
          icon:'i-heroicons-arrow-down-right'
        },
        {
          label: 'Categories',
          to:'/service/category',
          icon:'i-heroicons-arrow-down-right'
        },
      ]
    },
    {
        label: 'POS',
        icon: 'i-heroicons-calculator',
        to: '/pos'
    },
    {
        label: 'Quotations',
        icon: 'i-heroicons-document',
        to: '/quotations'
    },
    {
        label: 'Invoice',
        icon: 'i-heroicons-document-text',
        to: '/invoice'
    },
    {
        label: 'Customers',
        icon: 'i-heroicons-user-group',
        to: '/customers'
    },
    {
        label: 'Purchases',
        icon: 'i-heroicons-arrow-down',
        to: '/purchases'
    },
    {
        label: 'Expense',
        icon: 'i-heroicons-currency-dollar',
        to: '/expense'
    },
    {
        label: 'Srock Adjustment',  
        icon: 'i-heroicons-adjustments-horizontal',
        to: '/stock-adjusment'
    },
    {
        label: 'Reports',
        icon: 'i-heroicons-clipboard-document-list',
        children: [
          {
            label: 'Sales',
            icon:'i-heroicons-arrow-down-right',
            to: '/reports/sales'
          },
          {
            label: 'Profits / Loss',
            icon:'i-heroicons-arrow-down-right',
            to: '/reports/profits'
          },
        ]
    },
  ],
  [
    {
      label: 'Settings',
      icon: 'i-heroicons-cog-6-tooth',
      to: '/settings',
    }
  ] 
])
</script>

<template>
  <div>
    <UNavigationMenu :ui="{ list:'mb-3', link:'text-gray-700 dark:text-gray-300', linkLeadingIcon: 'text-gray-700 dark:text-gray-300'}" orientation="vertical" :items="items" class="data-[orientation=vertical]:w-52" />
  </div>
</template>
