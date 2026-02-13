<script setup lang="ts">
import { useTheme } from 'vuetify'
import { useAuthStore } from '@/stores/auth'
import { useRouter } from 'vue-router'
import { computed } from 'vue'
import { LogOut } from 'lucide-vue-next'

const theme = useTheme()
const authStore = useAuthStore()
const router = useRouter()

const isDark = computed(() => theme.global.current.value.dark)

function toggleTheme() {
  theme.global.name.value = theme.global.current.value.dark ? 'light' : 'dark'
}

function handleLogout() {
  authStore.logout()
  router.push('/login')
}
</script>

<template>
  <v-app-bar app>
    <v-app-bar-title>
      <router-link to="/product" class="text-decoration-none text-high-emphasis">
        E-Commerce
      </router-link>
    </v-app-bar-title>

    <v-spacer></v-spacer>

    <!-- Public Links -->
    <template v-if="!authStore.isAuthenticated">
      <v-btn to="/product" variant="text">Product</v-btn>
      <v-btn to="/login" variant="text">Login</v-btn>
      <v-btn to="/register" variant="text">Sign Up</v-btn>
    </template>

    <!-- Authenticated Links -->
    <template v-else>
      <v-btn to="/product" variant="text">Product</v-btn>
      <v-btn to="/orders" variant="text">Orders</v-btn>

      <v-menu>
        <template v-slot:activator="{ props }">
          <v-btn v-bind="props" icon>
            <LogOut class="w-5 h-5"/>
          </v-btn>
        </template>
        <v-list>
          <v-list-item>
            <v-list-item-title>Cart is empty</v-list-item-title>
          </v-list-item>
          <v-divider></v-divider>
          <v-list-item to="/cart">
            <v-list-item-title>View Cart</v-list-item-title>
          </v-list-item>
        </v-list>
      </v-menu>

      <v-btn @click="handleLogout" icon>
        <v-icon>mdi-logout</v-icon>
      </v-btn>
    </template>

    <v-btn icon @click="toggleTheme">
      <v-icon>{{ isDark ? 'mdi-weather-sunny' : 'mdi-weather-night' }}</v-icon>
    </v-btn>
  </v-app-bar>
</template>

<style scoped>
.text-high-emphasis {
  color: rgba(var(--v-theme-on-surface), var(--v-high-emphasis-opacity));
}
</style>
