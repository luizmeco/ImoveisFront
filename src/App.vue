<script setup lang="ts">
import { onMounted, ref } from 'vue'
import HeroSection from './components/HeroSection.vue'
import LoginDialog from './components/LoginDialog.vue'
import SiteHeader from './components/SiteHeader.vue'

interface Imovel {
  id: number
  titulo?: string | null
  descricao?: string | null
  finalidade?: string | null
  status?: string | null
  precoVenda?: number | string | null
  precoAluguel?: number | string | null
  dormitorios?: number | null
  banheiros?: number | null
  garagem?: number | null
  areaTotal?: number | string | null
  endereco?: string | null
  numero?: string | null
  bairro?: {
    nome?: string | null
    cidade?: string | null
    estado?: string | null
  } | null
  tipoImovel?: {
    nome?: string | null
  } | null
}

const isLoginOpen = ref(false)
const imoveis = ref<Imovel[]>([])
const isLoadingImoveis = ref(true)
const imoveisError = ref('')
const apiBaseUrl = (import.meta.env.VITE_API_BASE_URL || 'http://localhost:8081').replace(/\/$/, '')

onMounted(async () => {
  try {
    const response = await fetch(`${apiBaseUrl}/imoveis`)
    if (!response.ok) {
      throw new Error(`A API respondeu com o status ${response.status}.`)
    }

    const data: unknown = await response.json()
    if (!Array.isArray(data) || !data.every(isImovel)) {
      throw new Error('A resposta da API não contém uma lista válida de imóveis.')
    }

    imoveis.value = data
  } catch (error) {
    imoveisError.value =
      error instanceof Error
        ? `Não foi possível carregar os imóveis. ${error.message}`
        : 'Não foi possível carregar os imóveis. Verifique se a API está em execução.'
  } finally {
    isLoadingImoveis.value = false
  }
})

function isImovel(value: unknown): value is Imovel {
  return typeof value === 'object'
    && value !== null
    && 'id' in value
    && typeof value.id === 'number'
}

function formatPrice(value: number | string | null | undefined) {
  if (value === null || value === undefined || value === '') return null

  const numericValue = Number(value)
  if (!Number.isFinite(numericValue)) return null

  return new Intl.NumberFormat('pt-BR', {
    style: 'currency',
    currency: 'BRL',
    maximumFractionDigits: 0,
  }).format(numericValue)
}

function getPropertyPrice(imovel: Imovel) {
  const finalidade = imovel.finalidade?.toLocaleLowerCase('pt-BR') ?? ''
  const price = finalidade.includes('alug')
    ? formatPrice(imovel.precoAluguel) ?? formatPrice(imovel.precoVenda)
    : formatPrice(imovel.precoVenda) ?? formatPrice(imovel.precoAluguel)

  return price ?? 'Consulte valores'
}

function getLocation(imovel: Imovel) {
  const parts = [
    imovel.bairro?.nome,
    [imovel.bairro?.cidade, imovel.bairro?.estado].filter(Boolean).join(' - '),
  ].filter(Boolean)

  return parts.join(', ') || [imovel.endereco, imovel.numero].filter(Boolean).join(', ') || 'Localização não informada'
}
</script>

<template>
  <div class="page-shell">
    <SiteHeader @open-login="isLoginOpen = true" />

    <main>
      <HeroSection />

      <section id="catalogo" class="property-catalog" aria-labelledby="catalog-title">
        <div class="catalog-heading">
          <div>
            <p class="eyebrow"><span></span> SELEÇÃO CASA NOBRE</p>
            <h2 id="catalog-title" class="font-display">Imóveis para chamar de seu</h2>
          </div>
          <p v-if="!isLoadingImoveis && !imoveisError" class="catalog-count">
            {{ imoveis.length }} {{ imoveis.length === 1 ? 'imóvel encontrado' : 'imóveis encontrados' }}
          </p>
        </div>

        <p v-if="isLoadingImoveis" class="catalog-message" role="status">
          Carregando imóveis...
        </p>
        <p v-else-if="imoveisError" class="catalog-message catalog-message--error" role="alert">
          {{ imoveisError }}
        </p>
        <p v-else-if="imoveis.length === 0" class="catalog-message">
          Nenhum imóvel disponível no momento.
        </p>

        <div v-else class="property-grid">
          <article v-for="imovel in imoveis" :key="imovel.id" class="property-card">
            <div class="property-card__visual" aria-hidden="true">
              <span class="property-card__visual-mark">⌂</span>
              <span v-if="imovel.status" class="property-card__status">{{ imovel.status }}</span>
            </div>
            <div class="property-card__content">
              <p class="property-card__type">
                {{ imovel.tipoImovel?.nome || imovel.finalidade || 'Imóvel' }}
              </p>
              <h3>{{ imovel.titulo || 'Imóvel disponível' }}</h3>
              <p class="property-card__location">{{ getLocation(imovel) }}</p>
              <p v-if="imovel.descricao" class="property-card__description">{{ imovel.descricao }}</p>
              <p class="property-card__price">{{ getPropertyPrice(imovel) }}</p>
              <div class="property-card__details" aria-label="Características do imóvel">
                <span v-if="imovel.dormitorios != null">{{ imovel.dormitorios }} quartos</span>
                <span v-if="imovel.banheiros != null">{{ imovel.banheiros }} banheiros</span>
                <span v-if="imovel.garagem != null">{{ imovel.garagem }} vagas</span>
                <span v-if="imovel.areaTotal != null">{{ imovel.areaTotal }} m²</span>
              </div>
            </div>
          </article>
        </div>
      </section>

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
