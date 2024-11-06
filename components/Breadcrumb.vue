<script setup lang="ts">
const items = ref([
  {
    label: 'Dashbard',
    icon: 'i-heroicons-home',
    to: '/dashboard'
  },
  {
    label: props.prevPage.label,
    icon: props.prevPage.icon,
    to: props.prevPage.to
  },
  {
   label: props.currentPage.label,
    icon: props.currentPage.icon
  }
])
</script>

<template>
  <UBreadcrumb :items="items" />
</template>