import { h } from 'vue';
<script setup lang="ts">
const isOpen = ref(false);
const items = [
  [
    {
      slot: 'view',
    },  
    {
      slot: 'edit',
    }
  ],
  [{
    slot: 'delete',
  }]
]
//add service
const state = reactive({
  name:undefined,
  position:undefined,
});

const onSubmit = async () => {
  await execute();
  refresh()
  isOpen.value = false
  if(data.value){
    toast.add({ title: "Data Save Successfully Done..." });
    isOpen.value = false;
  }
  if(error.value){
    toast.add({ title:error.value.data.message, color:"red" })
  }
};
const {data, status, execute} = useAsyncData(
    async () => {
      return await $fetch(`/service`, {
        baseURL: useRuntimeConfig().public?.baseUrl,
        method: "POST",
        body:state,
        headers: {
          authorization: `Bearer ${useTokenStore().customer_token}`
        }
      });
    },
    {
      immediate: false,
    }
);


//Get Services
const { data: services, error, pending, refresh } = useLazyAsyncData(
    'services',
    () => $fetch( `/service`, {
      method: 'GET',
      baseURL: useRuntimeConfig().public.baseUrl,
      headers: {
        authorization: `Bearer ${useTokenStore().customer_token}`
      }
    }));

    //edit Service
    const isEdit = ref(false);
    const editState = reactive({
      id:undefined,
      name:undefined,
      position:undefined
    });

    const editService = (service: object) => {
      isEdit.value = true;
      editState.id = service?.id;
      editState.name = service?.name;
      editState.position = service?.position;
    }

    const onUpdate = async (id) => {
        await $fetch(`/service/${id}`, {
            baseURL: useRuntimeConfig().public?.baseUrl,
            method: "PATCH",
            body:editState,
            headers: {
              authorization: `Bearer ${useTokenStore().customer_token}`
            }
        });
        isEdit.value = false
        refresh()
      }

    //delete Service
    const deleteService = async (id) => {
      await $fetch(`/service/${id}`, {
              baseURL: useRuntimeConfig().public?.baseUrl,
              method: "DELETE",
              headers:{
                authorization: `Bearer ${useTokenStore().customer_token}`
              }
            });
            refresh()
      }

onMounted(() => {
    refresh()
  })
</script>

<template>
  <div class="lg:pl-5">
    <div class="flex items-center justify-between  pt-4 pb-8">
      <Heading>Services</Heading>
      <div class="flex items-center gap-3">
        <UInput
          icon="i-heroicons-magnifying-glass-20-solid"
          size="md"
          color="primary"
          :trailing="false"
          placeholder="Search..."
        />

        <UButton
          class="hidden lg:flex"
          icon="i-heroicons-plus"
          size="md"
          color="primary"
          variant="solid"
          label="Add Service"
          :trailing="false"
          @click="isOpen = true"
        />
      </div>
    </div>
    
    <div class="flex flex-wrap lg:-mx-3">
      <div class="w-full lg:w-1/4 lg:px-3 mb-4">
        <div class="rounded-xl p-4 relative bg-white/20 dark:bg-gray-900">
          <div class="absolute top-2 right-2">
            <UDropdown :items="items" :popper="{ placement: 'left-start' }" :ui="{'ring' : 'ring-primary'}">
              <UButton color="primary" variant="soft" trailing-icon="i-heroicons-sparkles" :ui="{'rounded' : 'rounded-full'}"  />
              <template #view="{ item }">
                    <UButton
                      icon="i-heroicons-eye"
                      size="sm"
                      color="primary"
                      variant="outline"
                      label="View"
                      :trailing="false"
                      block
                      :to="`/services/${service.id}`"
                    />
              </template>
              <template #edit="{ item }">
                <UButton
                      icon="i-heroicons-pencil-square"
                      size="sm"
                      color="primary"
                      variant="outline"
                      label="Edit"
                      :trailing="false"
                      block
                      @click="editService(service)"
                    />
              </template>
              <template #delete="{ item }">
                <UButton
                      icon="i-heroicons-trash"
                      size="sm"
                      color="primary"
                      variant="outline"
                      label="Delete"
                      :trailing="false"
                      @click="deleteService(service?.id)"
                      block
                    />
              </template>
            </UDropdown>
          </div>
          <NuxtLink to='`/services`' class="mt-2 mb-8 text-lg block text-white">Web Develpment</NuxtLink>
          <span class="glass py-1 px-3 rounded-full inline-block  text-sm">Position: 11</span>
        </div>
      </div>
    </div>
  </div>

<!-- Add Service -->
  <USlideover v-model:open="isOpen" >
    
    <template #content>
      <h3 class="text-2xl font-bold mb-4 p-3">Add Service</h3>
      <UForm :state="state" @submit="onSubmit" class="w-full flex flex-col gap-2 p-4">
        <UFormGroup label="Name" class="mb-5">
          <UInput placeholder="Service Name" color="primary" size="lg" v-model="state.name" class="w-full"  />
        </UFormGroup>
        <UFormGroup label="Position">
          <UInput placeholder="Service Position" color="primary" size="lg" v-model="state.position" class="w-full" />
        </UFormGroup>
        <div class="flex items-center gap-3 mt-5">
          <UButton
          size="lg"
          color="primary"
          variant="solid"
          label="Save"
          type="submit"
          />
          <UButton
          size="lg"
          color="primary"
          variant="outline"
          label="Cancel"
          type="reset"
          @click="isOpen = false"
          />
        </div>
      </UForm>
    </template>
  </USlideover>

  <!-- Edit Category -->
  <USlideover v-model:open="isEdit">
      <template #content>
        <UForm :state="editState" @submit="onUpdate(editState.id)">
        <UFormGroup label="Name" class="mb-5">
          <UInput placeholder="Service Name" color="primary" size="lg" v-model="editState.name"  />
        </UFormGroup>
        <UFormGroup label="Position">
          <UInput placeholder="Service Position" color="primary" size="lg" v-model="editState.position" />
        </UFormGroup>
        <div class="flex items-center gap-3 mt-5">
          <UButton
              size="lg"
              color="primary"
              variant="solid"
              label="Save Change"
              type="submit"
          />
          <UButton
              size="lg"
              color="primary"
              variant="outline"
              label="Cancel"
              type="reset"
              @click="isEdit = false"
          />
        </div>
      </UForm>
      </template>
  </USlideover>
</template>