<template>
  <v-card class="mx-auto" max-width="400" elevation="8" rounded="xl">
    <div class="header-gradient pa-8 text-center">
      <div class="header-decoration"></div>
      <div class="header-decoration-2"></div>
      <v-avatar size="64" class="mb-6 mail-icon-bg">
        <v-icon size="32" color="white">mdi-email-outline</v-icon>
      </v-avatar>
      <h1 class="text-h5 font-weight-bold mb-2 text-white">Envie seu Email</h1>
      <p class="text-body-2 text-blue-lighten-4 mb-0">Conecte-se conosco de forma rápida e segura</p>
    </div>

    <v-card-text class="pa-8">
      <v-form @submit.prevent="handleSubmit" ref="formRef">
        <v-text-field
          v-model="form.name"
          label="Nome Completo"
          placeholder="Digite seu nome completo"
          prepend-inner-icon="mdi-account-outline"
          variant="outlined"
          rounded="lg"
          required
          class="mb-2"
        />

        <v-text-field
          v-model="form.email"
          label="Endereço de Email"
          placeholder="seu@email.com"
          prepend-inner-icon="mdi-at"
          variant="outlined"
          rounded="lg"
          type="email"
          required
          class="mb-4"
        />

        <v-btn
          type="submit"
          color="blue"
          size="large"
          rounded="lg"
          block
          :loading="isSubmitting"
        >
          <v-icon start>mdi-send</v-icon>
          {{ isSubmitting ? 'Enviando...' : 'Enviar Email' }}
        </v-btn>
      </v-form>
    </v-card-text>
  </v-card>
</template>

<script setup>
import { reactive, ref } from 'vue'

const emit = defineEmits(['success', 'error'])

const formRef = ref(null)
const isSubmitting = ref(false)

const form = reactive({
  name: '',
  email: ''
})

const handleSubmit = async () => {
  const { valid } = await formRef.value.validate()

  if (!valid) {
    emit('error', 'Por favor, corrija os erros no formulário')
    return
  }

  isSubmitting.value = true

  try {
    const res = await fetch(`${import.meta.env.VITE_API_URL}/subscribers/`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(form),
    })


    if (!res.ok) throw new Error('Erro ao enviar')

    emit('success', 'Email enviado com sucesso!')
    form.name = ''
    form.email = ''
    formRef.value.reset()
  } catch (error) {
    console.error(error)
    emit('error', 'Erro ao enviar email. Tente novamente.')
  } finally {
    isSubmitting.value = false
  }
}
</script>

<style scoped>
/* Inclui os estilos decorativos do cabeçalho */
.header-gradient {
  background: linear-gradient(135deg, #3b82f6 0%, #2563eb 100%);
  position: relative;
  overflow: hidden;
}
.header-decoration {
  position: absolute;
  top: 16px;
  right: 16px;
  width: 80px;
  height: 80px;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 50%;
}
.header-decoration-2 {
  position: absolute;
  top: -32px;
  right: -32px;
  width: 128px;
  height: 128px;
  background: rgba(255, 255, 255, 0.05);
  border-radius: 50%;
}
.mail-icon-bg {
  background: rgba(255, 255, 255, 0.2) !important;
  backdrop-filter: blur(10px);
}
</style>
