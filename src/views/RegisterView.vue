<script setup lang="ts">
import { ref } from 'vue'
import { useAuthStore } from '@/stores/auth'
import { useRouter } from 'vue-router'

const name = ref('')
const email = ref('')
const password = ref('')
const confirmPassword = ref('')
const error = ref('')
const authStore = useAuthStore()
const router = useRouter()

async function handleRegister() {
  error.value = ''
  if (!name.value || !email.value || !password.value || !confirmPassword.value) {
    error.value = 'Please fill in all fields'
    return
  }

  if (password.value !== confirmPassword.value) {
    error.value = 'Passwords do not match'
    return
  }

  const success = await authStore.register(name.value ,email.value, password.value)
  if (success) {
    router.push('/login')
  } else {
    error.value = 'Registration failed'
  }
}



</script>

<template>
  <v-container class="fill-height" fluid>
    <v-row align="center" justify="center">
      <v-col cols="12" sm="8" md="4">
        <v-card class="elevation-12">
          <v-toolbar color="primary" dark flat>
            <v-toolbar-title>Register</v-toolbar-title>
          </v-toolbar>
          <v-card-text>
            <v-form @submit.prevent="handleRegister">
              <v-text-field
                v-model="name"
                label="name"
                name="name"
                type="text"
              ></v-text-field>

              <v-text-field
                v-model="email"
                label="email"
                name="email"
                type="text"
              ></v-text-field>

              <v-text-field
                v-model="password"
                id="password"
                label="Password"
                name="password"
                type="password"
              ></v-text-field>

              <v-text-field
                v-model="confirmPassword"
                id="confirmPassword"
                label="Confirm Password"
                name="confirmPassword"
                type="password"
              ></v-text-field>

              <v-alert v-if="error" type="error" dense class="mb-4">
                {{ error }}
              </v-alert>

              <v-btn type="submit" color="primary" block>Register</v-btn>
            </v-form>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn to="/login" color="secondary" text>Already have an account? Login</v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
