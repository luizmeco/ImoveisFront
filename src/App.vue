<script setup lang="ts">
import { ref } from 'vue'
import HeroSection from './components/HeroSection.vue'
import LoginDialog from './components/LoginDialog.vue'
import SiteHeader from './components/SiteHeader.vue'

const isLoginOpen = ref(false)
</script>

<template>
  <div class="page-shell">
    <SiteHeader @open-login="isLoginOpen = true" />

    <main>
      <HeroSection />

      <section id="sobre" class="trust-strip" aria-label="Sobre a Casa Nobre">
        <p class="font-display">Seu próximo capítulo começa com o lugar certo.</p>
        <div class="trust-details">
          <span><strong class="font-display">12 anos</strong> conectando pessoas e lares</span>
          <span><strong class="font-display">+2.400</strong> histórias que ganharam um novo endereço</span>
        </div>
      </section>
    </main>

    <footer class="site-footer">
      <a class="brand brand--footer" href="#inicio" aria-label="Casa Nobre, início">
        <span class="brand-mark" aria-hidden="true">⌂</span>
        <span>casa<span class="brand-accent">nobre</span></span>
      </a>
      <p>Encontrar um lar também é encontrar um jeito de viver.</p>
      <span>© 2026 Casa Nobre Imóveis</span>
    </footer>

    <LoginDialog v-if="isLoginOpen" @close="isLoginOpen = false" />
  </div>
</template>
