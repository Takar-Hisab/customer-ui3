<script setup lang="ts">
const colorMode = useColorMode();
const changeColor = () => (colorMode.preference = (colorMode.value === 'light' ? 'dark' : 'light'));
    definePageMeta({
        layout: 'pos',
    });
const currentTime = ref(new Date().toLocaleString())
const updateTime = () => {
  currentTime.value = new Date().toLocaleString()
}

const editProduct = ref<boolean>(false);

const isFullScreen = ref<boolean>(false);
function toggleFullScreen() {
  if (!isFullScreen.value) {
    if (document.documentElement.requestFullscreen) {
      document.documentElement.requestFullscreen();
    } else if (document.documentElement.webkitRequestFullscreen) { 
      document.documentElement.webkitRequestFullscreen();
    } else if (document.documentElement.msRequestFullscreen) { 
      document.documentElement.msRequestFullscreen();
    }
  } else {
    if (document.exitFullscreen) {
      document.exitFullscreen();
    } else if (document.webkitExitFullscreen) { 
      document.webkitExitFullscreen();
    } else if (document.msExitFullscreen) {
      document.msExitFullscreen();
    }
  }
  isFullScreen.value = !isFullScreen.value;
}

onMounted(() => {
  const interval = setInterval(updateTime, 1000)
  onUnmounted(() => clearInterval(interval))
})

 const {data: products} = useFetch('https://dummyjson.com/products?limit=100');

 const items = ref(['Backlog', 'Todo', 'In Progress', 'Walk-in Customer','Category', 'Brand']);
const value = ref('Walk-in Customer');
const value1 = ref('Category');
const value2 = ref('Brand');

const discValue = ref('')
const domains = ['%', 'flat',]
const domain = ref(domains[0])
</script>
<template>
    <div>
        <div class="flex items-center justify-between bg-white/30  dark:bg-gray-900/90 border-b border-slate-200 dark:border-b-gray-700 p-2 rounded-t-md ">
            <div class="flex items-center space-x-2">
                <UBadge>My Shop</UBadge>
                <UButton icon="i-heroicons-calendar-days" size='lg'>{{currentTime}}</UButton>
            </div>
            <div class="flex items-center space-x-2">
                <UButton size="sm" v-if="colorMode.value === 'light'" @click="changeColor" icon="i-heroicons-moon" label="Dark"/>
                <UButton size="sm" v-else @click="changeColor" icon="i-heroicons-sun" label="Light"/>
                <UButton @click="toggleFullScreen" size="sm" icon="i-heroicons-arrows-pointing-out">Full Screen</UButton>
                <UButton size="sm" icon="i-heroicons-folder-plus">Expense</UButton>
                <UButton size="sm" icon="i-heroicons-arrow-down-left">Sell Return</UButton>
                <UButton size="sm" icon="i-heroicons-x-mark" to="/dashboard">Close Terminal</UButton>
            </div>
        </div>
        <div class="flex">
            <div class="w-9/12">
                <div class="bg-white/20 dark:bg-gray-900/90 border border-t-0 border-r-0 border-slate-200 dark:border-gray-700  p-4 rounded-r-0 rounded-b-md p-2">
                    <div class="flex items-center gap-4">
                        <UInput :ui="{base:'trasparent'}" icon="i-heroicons-magnifying-glass" placeholder="Products/Sku/Barcode"
                        highlight
                        color="primary"
                         class="w-96" />
                         <USelect color="primary" highlight v-model="value1" :items="items" class="w-48" />
                         <USelect color="primary" highlight v-model="value2" :items="items" class="w-48" />
                    </div>
                    <div class="flex flex-wrap mt-4  h-[80vh] overflow-y-auto">
                        <div class="w-1/5 px-1 mb-2" v-for="product in products?.products">
                            <div class="bg-white/60 dark:bg-gray-900/80 border border-gray-50 dark:border-gray-700 p-2 rounded-md hover:border-primary h-full relative">
                                <div class="flex gap-2">
                                    <img class="w-12 h-12 rounded-md bg-white" :src="product?.thumbnail">
                                    <div>
                                        <h3 class="text-xs dark:text-gray-300">{{ product?.title }}</h3>
                                        <p class="text-base font-medium dark:text-gray-300">{{ product?.price }}</p>
                                        <UButton @click="editProduct = true" icon="i-heroicons-pencil-square" size="sm" variant="outline" class="absolute right-1 bottom-1" />
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
               
            </div>    
            <div class="w-3/12">
                <div class="bg-white/10 dark:bg-gray-900/90 border border-slate-300 dark:border-gray-700 rounded-l-md rounded-b-md">
                    <div class="border-b border-slate-300 dark:border-gray-700 p-2">
                        <USelect v-model="value" :items="items" class="w-full" size="lg" />
                    </div>
                    <div class="flex flex-col gap-2 h-[50vh] overflow-y-auto p-2">
                        <div class="border relative flex gap-2 bg-white/60 dark:bg-gray-900/90 border border-gray-50 dark:border-gray-700 rounded-md p-2" v-for="product in products?.products">
                                <img class="w-12 h-12 rounded-md bg-white" :src="product?.thumbnail">
                                <div class="w-full">
                                    <h3 class="text-xs font-medium mb-2">{{ product?.title }}</h3>
                                    <div class="flex items-center gap-2">
                                        <p class="text-base font-medium w-1/3">{{ product?.price }}</p>
                                        <div class="flex items-center gap-2">
                                            <UButton class=" top-1 right-1" size="xs" icon="i-heroicons-minus"  />
                                            <input type="text" value="1" class="text-center w-6 h-6 border border-primary rounded-md" readonly>
                                            <UButton class=" top-1 right-1" size="xs" icon="i-heroicons-plus" />
                                        </div>
                                    </div>
                                </div>
                                <div>
                                    <UButton class="absolute top-1 right-1" size="xs" icon="i-heroicons-trash" />
                                </div>
                        </div>
                    </div>
                    <div class="p-2 border-t border-slate-300 w-full bg-primary">
                        <UButtonGroup :ui="{base:'w-full'}">
                            <UInput
                            v-model="discValue"
                            :ui="{
                                base: 'pl-[70px] w-full',
                                leading: 'pointer-events-none w-full'
                            }"
                            >
                            <template #leading>
                                <p class="text-sm text-[var(--ui-text-muted)]">
                                Discount
                                </p>
                            </template>
                            </UInput>

                            <USelect v-model="domain" :items="domains"  />
                        </UButtonGroup>
                        <div class="flex items-center justify-between gap-2 py-2">
                            <UBadge color="neutral">Total Items: 10</UBadge>
                            <UBadge color="neutral">Discount: 00</UBadge>
                            <UBadge color="neutral">Sub Total: 100</UBadge>
                        </div>
                        <div class="bg-white dark:bg-gray-900 rounded-md p-3">
                            <div class="flex items-center justify-between mb-2">
                                <p class="text-base font-medium">Total Payable</p>    
                                <p class="text-base font-bold">1200</p>
                            </div>
                            <div class="flex items-center gap-2">
                                <div class="flex flex-col gap-2 w-2/5">
                                    <UButton 
                                  variant="outline"     
                                  >
                                    <img class="w-6 h-6 object-contain" src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSLf-e5EDjkZx6D3qfXoRo2Zakotq8KE--0tA&s">
                                    <span>Bkash</span>
                                  </UButton>
                                  <UButton 
                                  variant="outline"     
                                  >
                                    <img class="w-6 h-6 object-contain" src="https://freelogopng.com/images/1679248342nagad.png">
                                    <span>Nagad</span>
                                  </UButton>
                                </div>
                                <div class="w-3/5">
                                    <UButton :ui="{base:'w-full h-full flex flex-col items-center border border-dashed rounded-md', }" variant="outline">
                                        <UIcon name="i-heroicons-banknotes" class="text-4xl" />
                                        <span class="text-sm text-2xl font-medium">Cash</span>
                                    </UButton>  
                                </div>
                            </div>
                        </div>
                        <div class="bg-white dark:bg-gray-900 flex items-center gap-2 rounded-md p-2 mt-5">
                            <UButton size="sm" icon="i-heroicons-document">Quotation</UButton>
                            <UButton size="sm" icon="i-heroicons-x-mark">Cancel</UButton>
                        </div>
                    </div>
                </div>
            </div>    
        </div>
    </div>

 <!-- edit product modal -->
    <UModal  v-model:open="editProduct" title="Add Product">
        <template #body>
            <UForm class="space-y-4" @submit="onSubmit">
        <UFormField label="Product Name" name="name" class="w-full">
        <UInput  variant="subtle" size="xl" class="w-full" />
        </UFormField>

        <UFormField label="Product Price" name="name" class="w-full">
         <UInput  variant="subtle" size="xl" class="w-full" />
        </UFormField>
    
        <div class="flex items-center space-x-2">
            <UButton type="submit">Update Product</UButton>
        </div>
    </UForm>
        </template>
  </UModal>
</template>