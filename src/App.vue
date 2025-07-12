<template>
  <v-app>
    <v-main>
      <v-container class="fill-height d-flex align-center justify-center">
        <SubscriberForm @success="showSnackbar" @error="showSnackbarError" />
      </v-container>
    </v-main>

    <v-snackbar
      v-model="snackbar.show"
      :color="snackbar.color"
      :timeout="4000"
      location="top"
    >
      {{ snackbar.message }}
      <template v-slot:actions>
        <v-btn variant="text" @click="snackbar.show = false">Fechar</v-btn>
      </template>
    </v-snackbar>
  </v-app>
</template>

<script setup>
import { reactive } from 'vue'
import SubscriberForm from './components/SubscriberForm.vue'

const snackbar = reactive({
  show: false,
  message: '',
  color: 'success'
})

const showSnackbar = (message) => {
  snackbar.message = message
  snackbar.color = 'success'
  snackbar.show = true
}

const showSnackbarError = (message) => {
  snackbar.message = message
  snackbar.color = 'error'
  snackbar.show = true
}
</script>
