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
const isCartModal = ref(false);
const paymentItems = ref([
  {
    label: 'Cash',
	slot:'cash'
  },
  {
    label: 'MFC',
	slot:'mfc'
  },
  {
    label: 'Cerd',
	slot:'card'
  },
  {
    label: 'Credit',
	slot:'credit'
  }
])
</script>
<template>
	<!-- Mobile Nav -->
		<nav class="fixed left-0 right-0 bottom-0 bg-white p-1 z-50 md:hidden rounded-t-full shadow-xl">
			<ul class="flex items-center justify-around -mt-5">
				<li>
					<NuxtLink to="/dashboard" class="flex flex-col items-center gap-0.5">
						<Icon name="material-symbols:home-outline-rounded" class="text-2xl" />
						<span class="text-xs font-normal">Dashboard</span>
					</NuxtLink>
				</li>
				<li>
					<button :class="{'bg-primary text-white  shadow-lg' : !isCartModal}" @click="isCartModal = false" 
					 class="size-14 rounded-full flex flex-col items-center justify-center gap-0.5">
						<Icon name="mdi:post-outline" class="text-2xl" />
						<span class="text-xs font-normal">POS</span>
					</button>
				</li>
				<li>
					<button :class="{'bg-primary text-white  shadow-lg' : isCartModal}" class="size-14 rounded-full flex flex-col items-center justify-center gap-0.5" @click="isCartModal = !isCartModal">
						<Icon name="solar:cart-large-2-outline" class="text-2xl" />
						<span class="text-xs font-normal">Cart</span>
					</button>
				</li>
			</ul>
		</nav>
	<!-- Mobile Nav End -->
    <div>
        <div class="flex items-center justify-between bg-white/30  dark:bg-gray-900/90 border-b border-slate-200 dark:border-b-gray-700 p-2 rounded-t-md ">
			<div class="flex items-center space-x-2">
                <UBadge>My Shop</UBadge>
            </div>
			<div class="md:hidden">
				<UButton icon="material-symbols:menu-rounded" />
			</div>
            <div class="hidden lg:flex items-center space-x-2">
                <UButton size="sm" v-if="colorMode.value === 'light'" @click="changeColor" icon="i-heroicons-moon" label="Dark"/>
                <UButton size="sm" v-else @click="changeColor" icon="i-heroicons-sun" label="Light"/>
                <UButton @click="toggleFullScreen" size="sm" icon="i-heroicons-arrows-pointing-out">Full Screen</UButton>
                <UButton size="sm" icon="i-heroicons-folder-plus">Expense</UButton>
                <UButton size="sm" icon="i-heroicons-arrow-down-left">Sell Return</UButton>
                <UButton size="sm" icon="i-heroicons-x-mark" to="/dashboard">Close Terminal</UButton>
            </div>
        </div>
        <div class="flex">
            <div class="w-full md:w-2/3 lg:w-9/12 pb-20 md:pb-0">
                <div class="bg-white/20 dark:bg-gray-900/90 border border-t-0 border-r-0 border-slate-200 dark:border-gray-700 p-1 rounded-r-0 rounded-b-md">
                    <div class="flex items-center">
                        <UInput :ui="{base:'trasparent rounded-none'}" icon="i-heroicons-magnifying-glass" size="lg" placeholder="Products/Sku/Barcode"
                        color="primary"
                         class="w-1/2" />
                         <USelect :ui="{base:'trasparent rounded-none'}" color="primary" size="lg" v-model="value1" :items="items" class="w-1/4" />
                         <USelect :ui="{base:'trasparent rounded-none'}" color="primary" size="lg" v-model="value2" :items="items" class="w-1/4" />
                    </div>
                    <div class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 xl:grid-cols-5 gap-1 mt-1 h-[86vh] overflow-y-auto">
                        <div v-for="product in products?.products">
                            <button class="block w-full cursor-pointer bg-white/60 dark:bg-gray-900/80 border border-gray-50 dark:border-gray-700 hover:border-primary h-full overflow-hidden">
                                <div class="flex w-full h-full">
                                    <img class="w-16 object-contain h-full bg-white" :src="product?.thumbnail">
                                    <div class="p-2">
                                        <h3 class="text-xs text-black dark:text-gray-300 text-left">{{ product?.title }}</h3>
                                        <p class="text-base font-medium dark:text-gray-300 text-left">{{ product?.price }}</p>
                                    </div>
                                </div>
                            </button>
                        </div>
                    </div>
                </div>
            </div>    

            <div :class="{ 'z-40 opacity-100' :  isCartModal, '-z-50 md:z-0 md:opacity-100' : !isCartModal}" class="fixed md:static top-0 left-0 right-0 bottom-0 overflow-y-scroll pb-20 md:pb-0 h-screen md:h-auto w-full md:w-1/3 lg:w-3/12 transition-all ease-in-out duration-500">
                <div class="bg-white md:bg-white/10 dark:bg-gray-900/90 rounded-l-md rounded-b-md">
                    <div class="border-b border-slate-300 dark:border-gray-700">
                        <USelect v-model="value" :items="items" class="w-full rounded-none" size="lg" />
                    </div>
                    <div class="flex flex-col gap-1 md:h-[50vh] lg:h-[58vh] overflow-y-auto">
                        <div class="border border-gray-300 relative flex gap-2 bg-white/60 dark:bg-gray-900/90 border border-gray-50 dark:border-gray-700" v-for="product in products?.products">
                                <img class="w-16 h-full bg-white" :src="product?.thumbnail">
                                <div class="w-full">
                                    <h3 class="text-xs font-medium mb-2">{{ product?.title }}</h3>
                                    <div>
                                        <p class="text-base font-medium w-1/3">{{ product?.price }}</p>
                                    </div>
									<UInputNumber  />
                                </div>
                                <div>
                                    <UButton class="absolute top-1 right-1" size="xs" icon="i-heroicons-trash" />
                                </div>
                        </div>
                    </div>
                    <div class="fixed md:static bottom-16 md:bottom-0 shdaow w-full bg-white">
                        <div class="bg-white dark:bg-gray-900">
                            <div class="flex items-center justify-between p-1">
                                <p class="text-base font-normal">Total Payable</p>    
                                <p class="text-xl font-bold">1200</p>
                            </div>
                            <div class="border-y border-gray-400 p-1">
                                 <UTabs orientation="vertical"  :items="paymentItems" class="w-full">
									<template #cash="{item}">
										<div class="flex flex-col justify-between">  
										     <div class="flex flex-col gap-2 mb-2">
											 	<div class="flex items-center justify-between">
													<p class="text-sm font-normal">Collect</p>
													<UInput class="w-20" />
												</div>
												<div class="flex items-center justify-between">
													<p class="text-sm font-normal">Return</p>
													<UInput class="w-20" />
												</div>
											 </div>
											  <UButton label="Confirm Order" block />
										</div>
									</template>
									<template #mfc="{item}">
										<div class="flex flex-col justify-between w-full h-full">
											<ul class="grid grid-cols-2 lg:grid-cols-3 gap-2 mb-2">
												<li>
													<input type="radio" id="bkash" class="peer hidden" name="mfc">
													<label for="bkash" class="w-full flex items-center justify-center gap-1 font-normal py-2 border border-primary text-black peer-checked:text-white text-center peer-checked:bg-primary rounded-md text-white">
														<Icon name="arcticons:bkash" class="text-2xl block"  />
														<span class="text-xs font-normal">Bkash</span>
													</label>
												</li>
												<li>
													<input type="radio" id="nagad" class="peer hidden" name="mfc">
													<label for="nagad" class="w-full flex items-center justify-center gap-1 font-normal py-2 border border-primary text-black peer-checked:text-white text-center peer-checked:bg-primary rounded-md text-white">
														<Icon name="arcticons:nagad" class="text-2xl block"  />
														<span class="text-xs font-normal">Nagad</span>
													</label>
												</li>
												<li>
													<input type="radio" id="rocket" class="peer hidden" name="mfc">
													<label for="rocket" class="w-full flex items-center justify-center gap-1 font-normal py-2 border border-primary text-black peer-checked:text-white text-center peer-checked:bg-primary rounded-md text-white">
														<Icon name="mingcute:send-plane-line" class="text-2xl block"  />
														<span class="text-xs font-normal">Rocket</span>
													</label>
												</li>
												<li>
													<input type="radio" id="other-mfc" class="peer hidden" name="mfc">
													<label for="other-mfc" class="w-full flex items-center justify-center gap-1 font-normal py-2 border border-primary text-black peer-checked:text-white text-center peer-checked:bg-primary rounded-md text-white">
														<Icon name="material-symbols-light:payments-outline-rounded" class="text-2xl block"  />
														<span class="text-xs font-normal">Other</span>
													</label>
												</li>
											</ul>
										  <UButton label="Confirm Order" block />
										</div>
									</template>
									<template #card="{item}">
										<div class="flex flex-col justify-between w-full h-full">
											<ul class="grid grid-cols-2 lg:grid-cols-3 gap-2 mb-2">
												<li>
													<input type="radio" id="visa" class="peer hidden" name="card">
													<label for="visa" class="w-full flex gap-1 items-center justify-center font-normal py-2 border border-primary text-black peer-checked:text-white text-center peer-checked:bg-primary rounded-md text-white">
														<Icon name="brandico:visa" class="text-xl block"  />
														Visa
													</label>
												</li>
												<li>
													<input type="radio" id="master" class="peer hidden"  name="card">
													<label for="master"  class="w-full flex flex-col items-center justify-center font-normal  border border-primary text-black peer-checked:text-white text-center peer-checked:bg-primary rounded-md text-white">
														<Icon name="uit:master-card" class="text-2xl block"  />
														<span class="text-xs font-normal">Mastercard</span>
													</label>
												</li>
												<li>
													<input type="radio" id="ame-exp" class="peer hidden"  name="card">
													<label for="ame-exp"  class="w-full flex flex-col items-center justify-center font-normal  border border-primary text-black peer-checked:text-white text-center peer-checked:bg-primary rounded-md text-white">
														<Icon name="simple-icons:americanexpress" class="text-2xl block"  />
														<span class="text-xs font-normal">American Exp..</span>
													</label>
												</li>
												<li>
													<input type="radio" id="other-card" class="peer hidden"  name="card">
													<label for="other-card" class="w-full flex flex-col items-center justify-center font-normal  border border-primary text-black peer-checked:text-white text-center peer-checked:bg-primary rounded-md text-white">
														<Icon name="fluent:payment-16-regular" class="text-2xl block"  />
														<span class="text-xs font-normal">Other</span>
													</label>
												</li>
											</ul>
										  <UButton label="Confirm Order" block />
										</div>
									</template>
									<template #credit>
										<div>
											<UButton label="Confirm Order" block />
										</div>
									</template>
								 </UTabs>
                            </div>
                        </div>
                        <div class="bg-white dark:bg-gray-900 flex items-center gap-2 p-2">
                            <UButton size="sm" icon="i-heroicons-document">Quotation</UButton>
                            <UButton size="sm" icon="i-heroicons-x-mark">Cancel</UButton>
                        </div>
                    </div>
                </div>
            </div>    
        </div>
    </div>
</template>