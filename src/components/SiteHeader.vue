<script setup lang="ts">
defineEmits<{
  openLogin: []
}>()
</script>

<template>
  <header class="site-header">
    <a class="brand" href="#inicio" aria-label="Casa Nobre, início">
      <span class="brand-mark" aria-hidden="true">⌂</span>
      <span>casa<span class="brand-accent">nobre</span></span>
    </a>

    <nav class="main-nav" aria-label="Navegação principal">
      <a href="#inicio">Início</a>
      <a href="#catalogo">Imóveis</a>
      <a href="#sobre">Sobre nós</a>
    </nav>

    <button class="login-button" type="button" @click="$emit('openLogin')">
      Entrar
      <span aria-hidden="true">↗</span>
    </button>
  </header>
</template>
