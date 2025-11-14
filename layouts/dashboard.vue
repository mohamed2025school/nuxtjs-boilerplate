<template>
    <div class="min-h-screen flex bg-gray-100">
        <!-- SIDEBAR -->
        <transition name="slide">
            <aside :class="[
                'fixed inset-y-0 left-0 w-64 bg-white shadow-lg z-50 flex flex-col transform md:translate-x-0 md:relative',
                isSidebarOpen ? 'translate-x-0' : '-translate-x-full'
            ]">
                <div class="flex items-center justify-between h-16 border-b px-4">
                    <h1 class="text-xl font-bold text-gray-800">MyDashboard</h1>
                    <button @click="toggleSidebar" class="md:hidden p-2 rounded hover:bg-gray-100">
                        <Icon icon="mdi:close" class="text-2xl text-gray-600" />
                    </button>
                </div>
                <nav class="flex-1 px-4 py-6 space-y-2 overflow-y-auto select-none">

                    <!-- Dashboard -->
                    <NuxtLink to="/dashboard"
                        class="flex items-center gap-3 px-3 py-2 rounded-lg hover:bg-gray-100 transition submenu-link font-mono "
                        active-class="bg-gray-200 font-semibold submenu-link">
                        <Icon icon="mdi:view-dashboard-outline" class="text-xl" />
                        About Us
                    </NuxtLink>

                    <MenuSidebar title="POS" icon="mdi:account-outline" :menu="menu_pos" />
                    <!-- Settings -->
                    <NuxtLink to="/dashboard/settings"
                        class="flex items-center gap-3 px-3 py-2 rounded-lg hover:bg-gray-100 transition submenu-link font-mono"
                        active-class="bg-gray-200 font-semibold">
                        <Icon icon="mdi:cog-outline" class="text-xl" />
                        Settings
                    </NuxtLink>

                </nav>

            </aside>
        </transition>

        <!-- OVERLAY (mobile) -->
        <div v-if="isSidebarOpen" class="fixed inset-0 bg-black/40 z-40 md:hidden" @click="toggleSidebar"></div>

        <!-- MAIN CONTENT -->
        <div class="flex-1 flex flex-col  transition-all">
            <!-- HEADER -->
            <header class="h-16 bg-cip-700  shadow flex items-center justify-between px-4 md:px-6 sticky top-0 z-30">
                <div class="flex items-center gap-3">
                    <span @click="toggleSidebar" class="md:hidden p-2 cursor-pointer">
                        <Icon icon="mdi:menu" class="text-2xl text-white" />
                    </span>
                    <h2 class="text-lg font-semibold text-white">{{ pageTitle }}</h2>
                </div>

                <div class="flex items-center gap-4">
                    <span class="p-2 rounded-full bg-white hover:bg-gray-100">
                        <Icon icon="mdi:bell-outline" class="text-xl text-gray-600" />
                    </span>
                    <!-- <img src="/Logo.avif" alt="User Avatar" class="w-10 h-10 rounded-full border bg-white" /> -->
                    <div class="relative" @click.stop="toggleUserMenu">
                        <img src="/Logo.avif" class="w-10 h-10 rounded-full border cursor-pointer bg-white" />

                        <!-- DROPDOWN -->
                        <transition name="fade">
                            <div v-if="userMenuOpen"
                                class="absolute right-0 mt-2 w-48 bg-white rounded-xl shadow-lg border py-2 z-50">
                                <span
                                    class="flex items-center gap-2 px-4 py-2 hover:bg-gray-100 w-full text-left cursor-pointer">
                                    <Icon icon="mdi:account-circle-outline" class="text-xl" />
                                    Profile
                                </span>

                                <span
                                    class="flex items-center gap-2 px-4 py-2 hover:bg-gray-100 w-full text-left cursor-pointer">
                                    <Icon icon="mdi:cog-outline" class="text-xl" />
                                    Settings
                                </span>

                                <span
                                    class="flex items-center gap-2 px-4 py-2 hover:bg-gray-100 w-full text-left text-red-600 cursor-pointer">
                                    <Icon icon="mdi:logout" class="text-xl" />
                                    Logout
                                </span>
                            </div>
                        </transition>
                    </div>
                </div>
            </header>

            <!-- CONTENT -->
            <main class="p-4 md:p-6 flex-1">
                <slot />
            </main>
        </div>
    </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { Icon } from '@iconify/vue'
import { useRoute } from 'vue-router'

const route = useRoute()
const isSidebarOpen = ref(false)

const toggleSidebar = () => {
    isSidebarOpen.value = !isSidebarOpen.value
}

const userMenuOpen = ref(false)

const menu_pos = [
    {
        title: "General Manager",
        link: "/dashboard/pos/general-manager",
        icon: "mdi:account-outline"
    },
    {
        title: "Assistant General Manager",
        link: "/dashboard/pos/assistant-general-manager",
        icon: "mdi:account-outline"
    },
    {
        title: "Manager",
        link: "/dashboard/pos/manager",
        icon: "mdi:account-outline"
    },
    {
        title: "Assistant Manager",
        link: "/dashboard/pos/assistant-manager",
        icon: "mdi:account-outline"
    },
    {
        title: "Restaurant Supervisor",
        link: "/dashboard/pos/restaurant-supervisor",
        icon: "mdi:account-outline"
    },
    {
        title: "Head Waiter",
        link: "/dashboard/pos/head-waiter",
        icon: "mdi:account-outline"
    },
    {
        title: "Chef De Rang",
        link: "/dashboard/pos/chef-de-rang",
        icon: "mdi:account-outline"
    },
    {
        title: "Runner",
        link: "/dashboard/pos/runner",
        icon: "mdi:account-outline"
    },
    {
        title: "Reception Manager",
        link: "/dashboard/pos/reception-manager",
        icon: "mdi:account-outline"
    },
    {
        title: "Hostess",
        link: "/dashboard/pos/hostess",
        icon: "mdi:account-outline"
    }
]

const toggleUserMenu = () => {
    userMenuOpen.value = !userMenuOpen.value
}

// close dropdown when clicking outside
onMounted(() => {
    document.addEventListener("click", () => {
        userMenuOpen.value = false
    })
})

const pageTitle = computed(() =>
    route.name ? route.name.toString().replace('dashboard-', '').toUpperCase() : 'Dashboard'
)
</script>

<style scoped>
.slide-enter-active,
.slide-leave-active {
    transition: transform 0.3s ease;
}

.slide-enter-from,
.slide-leave-to {
    transform: translateX(-100%);
}

.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.15s ease;
}

.fade-enter-from,
.fade-leave-to {
    opacity: 0;
}
</style>
