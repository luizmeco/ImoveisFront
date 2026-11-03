<script setup lang="ts">
import { ref } from 'vue'

defineEmits<{
  close: []
}>()

const isSubmitted = ref(false)

function handleSubmit() {
  isSubmitted.value = true
}
</script>

<template>
  <div class="dialog-backdrop" @click.self="$emit('close')" @keydown.esc="$emit('close')">
    <section
      class="login-dialog"
      role="dialog"
      aria-modal="true"
      aria-labelledby="login-title"
    >
      <button class="dialog-close" type="button" aria-label="Fechar" @click="$emit('close')">
        ×
      </button>
      <p class="eyebrow"><span></span> BEM-VINDO DE VOLTA</p>
      <h2 id="login-title" class="font-display">Entre na sua conta</h2>
      <p class="dialog-description">Acompanhe seus imóveis e conversas em um só lugar.</p>

      <form class="login-form" @submit.prevent="handleSubmit">
        <label for="email">E-mail</label>
        <input id="email" type="email" autocomplete="email" placeholder="voce@email.com" required />
        <label for="password">Senha</label>
        <input
          id="password"
          type="password"
          autocomplete="current-password"
          placeholder="Sua senha"
          required
        />
        <button class="primary-button login-submit" type="submit">Entrar na conta</button>
      </form>

      <p v-if="isSubmitted" class="demo-notice" role="status">
        Esta é uma demonstração. A autenticação ainda não está conectada.
      </p>
      <p v-else class="demo-notice">Acesso demonstrativo — autenticação ainda não conectada.</p>
    </section>
  </div>
</template>
