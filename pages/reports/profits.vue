<script setup lang=ts>
    const items = ref([
    {
        label: 'Dashbard',
        icon: 'i-heroicons-home',
        to: '/dashboard'
    },
    {
        label: 'Reports',
        icon: 'i-heroicons-cube',
    },
    {
        label: 'Profits',
        icon: 'i-heroicons-link',
    },
]);
</script>

<template>
<div class="py-4">
    <UBreadcrumb  :items="items" />
</div>
<div class="bg-white/10 border border-slate-200 dark:border-gray-700 dark:bg-gray-900/80 p-2 rounded-lg  shadow mt-3">
    <h3 class="text-lg font-bold mb-2">Profits</h3>
                    <BarChart />
                </div>
</template>