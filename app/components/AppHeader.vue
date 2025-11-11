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
  <UHeader class="ui-bg-nav bg-[#D8CFC4] text-black" mode="slideover">
    <template #left>
      <NuxtLink to="/">
        <AppLogo class="w-auto h-6 shrink-0 text-black text-lg" />
      </NuxtLink>

    </template>

    <UNavigationMenu :items="items" variant="pill" color="primary"  class="text-xl text-black" :ui="{
      link: 'group relative w-full flex items-center gap-1.5 text-black font-medium text-sm before:absolute before:z-[-1] before:rounded-md focus:outline-none focus-visible:outline-none dark:focus-visible:outline-none focus-visible:before:ring-inset focus-visible:before:ring-2',
      linkLabelExternalIcon: 'inline-block size-3 align-top text-black',
      linkLeadingIcon: 'text-black'
    }" />

    <template #body>
      <UNavigationMenu :items="items" orientation="vertical" class="-mx-2.5 text-3xl text-black" />
    </template>
  </UHeader>
</template>
