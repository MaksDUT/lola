<script setup lang="ts">
const route = useRoute()

const items = computed(() => [{
  label: 'Accueil',
  to: '/',

}, {
  label: 'Formules',
  to: '/formules',

},

{
  label: 'Découvrez-nous',
  to: '/a-propos',
  class: ""
},

{
  label: 'FAQ',
  to: '/faq',

},

{
  label: 'Contact',
  to: '/contact',

},

{
  label: 'Blog',
  to: '/blog',
},

{
  label: '',
  icon: 'i-lucide-calendar',
  to: '/rdv',
}

])
</script>

<template>
  <UHeader class="ui-bg-nav bg-[#D8CFC4] text-black">
    <template #left>
      <NuxtLink to="/">
        <AppLogo class="w-auto h-6 shrink-0 text-black text-lg" />
      </NuxtLink>

    </template>

    <UNavigationMenu :items="items" variant="pill" color="secondary" class="text-xl text-black"  />

    <template #body>
      <UNavigationMenu :items="items" orientation="vertical" class="-mx-2.5 text-3xl" />
    </template>
  </UHeader>
</template>
