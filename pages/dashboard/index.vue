<script setup lang="ts">
const categoryModal = ref<boolean>(false);
const brandModal = ref<boolean>(false);
const productModal = ref<boolean>(false);
</script>
<template>
    <div class=" w-full">
        <div class="flex">
            <div class="w-2/3 ">
                <div class="w-full flex gap-2">
                    <div  class="flex space-x-2">
                   <div class="flex flex-col items-center justify-center  p-3 h-full shadow-2xl border-slate-200 rounded-lg bg-primary min-w-44">
                        <p class="text-gray-100 text-base">Total Sales</p>
                        <div class="w-24 h-24 backdrop-blur-sm bg-gray-100/20 rounded-full border border-slate-200/40 flex items-center justify-center my-2">
                            <UIcon name="i-heroicons-banknotes" class="text-3xl text-white" />
                        </div>
                        <h3 class="text-gray-100 text-xl font-bold">$20, 000, 000</h3>
                   </div>

                   <div class="flex flex-col items-center justify-center  p-3 h-full shadow-2xl border-slate-200 rounded-lg  bg-primary min-w-44">
                        <p class="text-gray-100 text-base">Total purchase</p>
                        <div class="w-24 h-24 backdrop-blur-sm bg-gray-100/20 rounded-full border border-slate-200/40 flex items-center justify-center my-2">
                            <UIcon name="i-heroicons-arrow-up-right" class="text-3xl text-white" />
                        </div>
                        <h3 class="text-gray-100 text-xl font-bold">$20, 000, 000</h3>
                   </div>
                </div>
                <div class="w-full">
                    <div class="w-full bg-white/10 border border-slate-200 p-2 py-3 rounded-lg  shadow flex flex-wrap dark:border-gray-700  dark:bg-gray-900/80 ">
                        <div class="w-1/2 px-1 mb-2">
                        <div class="w-full bg-white/10 border border-slate-200 dark:border-gray-700 dark:bg-gray-900/80 rounded-lg shadow p-2">
                            <p class="text-gray-800 dark:text-gray-300 text-sm">Total Categories</p>
                            <UIcon name="i-heroicons-sparkles" class="text-2xl text-gray-800 dark:text-gray-300" />
                            <div class="flex items-center justify-between">
                                <h3 class="text-gray-800 dark:text-gray-300">100</h3>
                                <UButton  @click="categoryModal = !categoryModal" size="sm" variant="subtle">Add</UButton>
                            </div>
                        </div>
                    </div>
                    <div class="w-1/2 px-1 mb-2">
                        <div class="w-full bg-white/10 border border-slate-200 dark:border-gray-700 dark:bg-gray-900/80 rounded-lg shadow p-2">
                            <p class="text-gray-800 dark:text-gray-300 text-sm">Total Brands</p>
                            <UIcon name="i-heroicons-sparkles" class="text-2xl text-gray-800 dark:text-gray-300" />
                            <div class="flex items-center justify-between">
                                <h3 class="text-gray-800 dark:text-gray-300">50</h3>
                                <UButton size="sm" variant="subtle" @click="brandModal = !brandModal">Add</UButton>
                            </div>
                        </div>
                    </div>
                    <div class="w-full px-1">
                        <div class="w-full bg-white/10 border border-slate-200 dark:border-gray-700 dark:bg-gray-900/80 rounded-lg shadow p-2">
                            <p class="text-gray-900 dark:text-gray-300 text-sm">Total Products</p>
                            <UIcon name="i-heroicons-sparkles" class="text-2xl text-gray-800 dark:text-gray-300" />
                            <div class="flex items-center justify-between">
                                <h3 class="text-gray-900 dark:text-gray-300">350</h3>
                                <UButton size="sm" variant="subtle" @click="productModal = !productModal">Add</UButton>
                            </div>
                        </div>
                    </div>
                    </div>
                    
                </div>
                </div>
                <div class="bg-white/10 border border-slate-200 dark:border-gray-700 dark:bg-gray-900/80 p-2 rounded-lg  shadow mt-3">
                    <BarChart />
                </div>
            </div>
            <div class="w-1/3 pl-2">
                <div class="w-full bg-white/10 border border-slate-200 dark:border-gray-700 dark:bg-gray-900/80 p-2 rounded-lg flex flex-col gap-2">
                    <div class="bg-white/10 border border-slate-200 dark:border-gray-700 dark:bg-gray-900/80 rounded-lg shadow p-2 flex gap-3">
                        <span class=" border border-slate-300  dark:border-gray-700 dark:bg-gray-900/80  w-10 h-10 rounded-md flex items-center justify-center bg-primary">
                            <UIcon name="i-heroicons-bars-arrow-up" class="text-white dark:text-gray-300 text-2xl"/>
                        </span>
                        <div>
                            <p class="text-sm font-medium  text-gray-900 dark:text-gray-300">Expense</p>
                            <h3 class="font-bold dark:text-gray-300">$00, 00</h3>
                        </div>
                    </div>

                    <div class="bg-white/10 border border-slate-200 dark:border-gray-700 dark:bg-gray-900/80 rounded-lg shadow p-2 flex gap-3">
                        <span class=" border border-slate-300 dark:border-gray-700 dark:bg-gray-900/80  w-10 h-10 rounded-md flex items-center justify-center bg-primary">
                            <UIcon name="i-heroicons-bars-arrow-up" class="text-white dark:text-gray-300 text-2xl"/>
                        </span>
                        <div>
                            <p class="text-sm  font-medium  text-gray-900 dark:text-gray-300">Total Sell Return</p>
                            <h3 class="font-bold dark:text-gray-300">$00, 00</h3>
                        </div>
                    </div>


                    <div class="bg-white/10 border border-slate-200 dark:border-gray-700 dark:bg-gray-900/80 rounded-lg shadow p-2 flex gap-3">
                        <span class=" border border-slate-300 dark:border-gray-700 dark:bg-gray-900/80  w-10 h-10 rounded-md flex items-center justify-center bg-primary">
                            <UIcon name="i-heroicons-bars-arrow-up" class="text-white dark:text-gray-300 text-2xl"/>
                        </span>
                        <div>
                            <p class="text-sm  font-medium  text-gray-900 dark:text-gray-300">Total Purchase Return</p>
                            <h3 class="font-bold">$00, 00</h3>
                        </div>
                    </div>

                    <div class="bg-white/10 border border-slate-200 dark:border-gray-700 dark:bg-gray-900/80 rounded-lg shadow p-2 flex gap-3">
                        <span class=" border border-slate-300 dark:border-gray-700 dark:bg-gray-900/80  w-10 h-10 rounded-md flex items-center justify-center bg-primary">
                            <UIcon name="i-heroicons-bars-arrow-up" class="text-white dark:text-gray-300 text-2xl"/>
                        </span>
                        <div>
                            <p class="text-sm  font-medium  text-gray-900 dark:text-gray-300">Invoice due</p>
                            <h3 class="font-bold dark:text-gray-300">$00, 00</h3>
                        </div>
                    </div>

                    <div class="bg-white/10 border border-slate-200 dark:border-gray-700 dark:bg-gray-900/80 rounded-lg shadow p-2 flex gap-3">
                        <span class=" border border-slate-300 dark:border-gray-700 dark:bg-gray-900/80  w-10 h-10 rounded-md flex items-center justify-center bg-primary">
                            <UIcon name="i-heroicons-bars-arrow-up" class="text-white text-2xl dark:text-gray-300"/>
                        </span>
                        <div>
                            <p class="text-sm  font-medium  text-gray-900 dark:text-gray-300">Purchase due</p>
                            <h3 class="font-bold">$00, 00</h3>
                        </div>
                    </div>
                </div>


                <div class="w-full bg-white/10 border border-slate-200 dark:border-gray-700 dark:bg-gray-900/80 p-2 rounded-lg flex flex-col gap-2 mt-3">
                    <h3 class="font-medium  text-base dark:text-gray-300">Top Products</h3>
                    <div class="bg-white/10 border border-slate-200 dark:border-gray-700 dark:bg-gray-900/80 rounded-lg shadow p-2 flex gap-3">
                        <img class="w-16 h-16 rounded" src="https://img.drz.lazcdn.com/static/bd/p/84e96a44fa7d334e45938668684c445f.jpg_400x400q75.jpg_.webp">
                        <div>
                            <h3 class="text-sm font-medium dark:text-gray-300">Samsung Galaxy S24 Ultra 5G 12GB..</h3>
                            <p class="text-sm">Price: 120</p>
                            <UBadge>Stock: 10</UBadge>
                        </div>
                    </div>

                    <div class="bg-white/10 border border-slate-200 dark:border-gray-700 dark:bg-gray-900/80 rounded-lg shadow p-2 flex gap-3">
                        <img class="w-16 h-16 rounded" src="https://img.drz.lazcdn.com/g/kf/S65be4551baf549dbb82225273137aeedJ.jpg_400x400q75.jpg_.webp">
                        <div>
                            <h3 class="text-sm font-medium dark:text-gray-300">AiQUE Baby Digital Infrared Forehead..</h3>
                            <p class="text-sm">Price: 120</p>
                            <UBadge>Stock: 10</UBadge>
                        </div>
                    </div>

                    <div class="bg-white/10 border border-slate-200 dark:border-gray-700 dark:bg-gray-900/80 rounded-lg shadow p-2 flex gap-3">
                        <img class="w-16 h-16 rounded" src="https://img.drz.lazcdn.com/static/bd/p/d7570ec09f612b721a08219596d6c455.jpg_400x400q75.jpg_.webp">
                        <div>
                            <h3 class="text-sm font-medium dark:text-gray-300">T900 Ultra 2 Men Smart Watch 2.3"...</h3>
                            <p class="text-sm">Price: 120</p>
                            <UBadge>Stock: 10</UBadge>
                        </div>
                    </div>
                    <div class="bg-white/10 border border-slate-200 dark:border-gray-700 dark:bg-gray-900/80 rounded-lg shadow p-2 flex gap-3">
                        <img class="w-16 h-16 rounded" src="https://img.drz.lazcdn.com/static/bd/p/2be9fb86bb0ef2cbec1ef376fc6d05fd.png_400x400q75.png_.webp">
                        <div>
                            <h3 class="text-sm font-medium dark:text-gray-300">iPhone 16 Global</h3>
                            <p class="text-sm">Price: 120</p>
                            <UBadge>Stock: 10</UBadge>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>




    <!-- // create categort -->
    <UModal  v-model:open="categoryModal" title="Add Category">
        <template #body>
            <UForm class="space-y-4" @submit="onSubmit">
        <UFormField label="Category Name" name="name" class="w-full">
        <UInput  variant="subtle" size="xl" class="w-full">
        </UInput>
        </UFormField>

        
        <UFormField label="Category Image" name="image" class="w-full">
            <UInput type="file" variant="subtle" size="xl" />
        </UFormField>
        

        <div class="flex items-center space-x-2">
            <UButton type="submit">Save Category</UButton>
        </div>
    </UForm>
        </template>
  </UModal>
  
<!-- //create brand -->
  <UModal  v-model:open="brandModal" title="Add Brand">
        <template #body>
            <UForm class="space-y-4" @submit="onSubmit">
        <UFormField label="Brand Name" name="name" class="w-full">
        <UInput  variant="subtle" size="xl" class="w-full">
        </UInput>
        </UFormField>

        
        <UFormField label="Brand Image" name="image" class="w-full">
            <UInput type="file" variant="subtle" size="xl" />
        </UFormField>
        

        <div class="flex items-center space-x-2">
            <UButton type="submit">Save Brand</UButton>
        </div>
    </UForm>
        </template>
  </UModal>

  <!-- //create Product -->
  <UModal  v-model:open="productModal" title="Add Product">
        <template #body>
            <UForm class="space-y-4" @submit="onSubmit">
        <UFormField label="Product Name" name="name" class="w-full">
        <UInput  variant="subtle" size="xl" class="w-full" />
        </UFormField>

        <UFormField label="Product Price" name="name" class="w-full">
         <UInput  variant="subtle" size="xl" class="w-full" />
        </UFormField>
    
        <div class="flex items-center space-x-2">
            <UButton type="submit">Save Product</UButton>
        </div>
    </UForm>
        </template>
  </UModal>
</template>