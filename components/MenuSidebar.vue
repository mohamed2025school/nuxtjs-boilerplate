<template>
  <div class="select-none">
    <!-- main link -->
    <span
      @click="toggleUsersMenu"
      class="flex items-center justify-between w-full px-3 py-2 rounded-lg hover:bg-gray-100 transition submenu-link font-mono"
    >
      <div class="flex items-center gap-3 font-mono">
        <Icon :icon="icon" class="text-xl" />
        {{ title }}
      </div>

      <Icon :icon="usersMenuOpen ? 'mdi:chevron-up' : 'mdi:chevron-down'" class="text-xl" />
    </span>

    <!-- submenu -->
    <transition name="fade">
      <div v-if="usersMenuOpen" class="ml-5 mt-2 space-y-2">
        <NuxtLink
          v-for="item in menu"
          :key="item.link"
          :to="item.link"
          class="block px-2 py-1 hover:bg-gray-100 rounded submenu-link font-mono"
          active-class="bg-gray-200 font-semibold"
        >
          <div class="flex items-center gap-2 font-mono">
            <Icon v-if="item.icon" :icon="item.icon" class="text-lg" />
            {{ item.title }}
          </div>
        </NuxtLink>
      </div>
    </transition>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { Icon } from '@iconify/vue'

const props = defineProps({
  title: {
    type: String,
    default: ""
  },
  icon: {
    type: String,
    default: ""
  },
  menu: {
    type: Array,
    default: () => []      // هادي أهم حاجة!
  }
})

const usersMenuOpen = ref(false)

const toggleUsersMenu = () => {
  usersMenuOpen.value = !usersMenuOpen.value
}
</script>
