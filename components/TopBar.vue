<script setup lang="ts">
const colorMode = useColorMode();
const changeColor = () => (colorMode.preference = (colorMode.value === 'light' ? 'dark' : 'light'));
</script>
<template>
    <div class="flex items-center justify-between  px-20 py-3">
        <h3 class="text-2xl font-bold italic text-primary">Takar Hisab</h3>
       
        <ul class="flex items-center space-x-3">
          <li>
            <UButton
                  icon="i-heroicons-calculator"
                  to="/pos"
                >
                  POS
                </UButton>
          </li>
            <li>
              <UButton @click="changeColor" size="lg">
                <UIcon name="i-heroicons-sun" v-if="colorMode.value === 'dark'"  />
                <UIcon name="i-heroicons-moon" v-else  />
              </UButton>
            </li>
            <li>
              <UButton
                  icon="i-heroicons-bell"
                />
            </li>
            <li>
              <UDropdownMenu :items="dropDownItems" class="w-48">
                <UAvatar src="https://github.com/benjamincanac.png" />
              </UDropdownMenu>
            </li>
        </ul>
    </div>
</template>