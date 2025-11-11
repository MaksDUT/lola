<script setup lang="ts">
const route = useRoute()

const { data: post } = await useAsyncData(route.path, () => queryCollection('formule').path(route.path).first())
if (!post.value) {
  throw createError({ statusCode: 404, statusMessage: 'Post not found', fatal: true })
}

const { data: surround } = await useAsyncData(`${route.path}-surround`, () => {
  return queryCollectionItemSurroundings('posts', route.path, {
    fields: ['description']
  })
})

const title = post.value.seo?.title || post.value.formule.title
const description = post.value.seo?.description || post.value.description

useSeoMeta({
  title,
  ogTitle: title,
  description,
  ogDescription: description
})

const features = ref([
  {
    title: 'Icons',
    description: 'Nuxt UI integrates with Nuxt Icon to access over 200,000+ icons from Iconify.',
    icon: 'i-lucide-smile',
    to: '/docs/getting-started/integrations/icons'
  },
  {
    title: 'Fonts',
    description: 'Nuxt UI integrates with Nuxt Fonts to provide plug-and-play font optimization.',
    icon: 'i-lucide-a-large-small',
    to: '/docs/getting-started/integrations/fonts'
  },
  {
    title: 'Color Mode',
    description: 'Nuxt UI integrates with Nuxt Color Mode to switch between light and dark.',
    icon: 'i-lucide-sun-moon',
    to: '/docs/getting-started/integrations/color-mode'
  }
])

const links = ref([
  {
    label: 'Explore components',
    to: '/docs/components/app',
    color: 'neutral',
    variant: 'subtle',
    trailingIcon: 'i-lucide-arrow-right'
  }
])
</script>

<template>
  <UContainer v-if="post">

    <UPageSection orientation="horizontal">
      <template #header>
        <UPricingPlan :title="post.formule.title" :description="post.formule.description" :price="post.formule.price"
          :button="{
            label: 'Prendre rendez-vous'
          }" :ui="{
            titleWrapper: 'flex items-center gap-3 justify-center',
            description: 'text-muted text-base text-pretty mt-2 text-justify'
          }">


          <template #features>
            <h4 class="text-muted text-sm">Inclus </h4>
            <ul class="flex flex-col gap-3 flex-1 grow-0">
              <li class="flex items-center gap-2 min-w-0" v-for="value in post.formule.features">
                <svg xmlns="http://www.w3.org/2000/svg" class="size-5 shrink-0 text-primary" width="24" height="24" viewBox="0 0 24 24"><g fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"><circle cx="12" cy="12" r="10"/><path d="m9 12l2 2l4-4"/></g></svg>
                <span
                  class="text-muted text-sm ">{{ value }}
                </span>
              </li>
            </ul>

            <h4 class="text-muted text-sm">Non inclus </h4>

            <ul class="flex flex-col gap-3 flex-1 grow-0">
              <li class="flex items-center gap-2 min-w-0" v-for="value in post.formule.features">
                
                <svg xmlns="http://www.w3.org/2000/svg" class="size-5 shrink-0 text-error" width="24" height="24" viewBox="0 0 24 24"><g fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"><circle cx="12" cy="12" r="10"/><path d="m15 9l-6 6m0-6l6 6"/></g></svg>

                <span class="text-muted text-sm  text-error">{{ value }}
                </span>
              </li>
            </ul>

          </template>

        </UPricingPlan>
      </template>
      <NuxtImg :src="post.formule.image.src" width="3000" height="4000" :alt="post.formule.title"
        class="w-full h-150 rounded-lg" />
    </UPageSection>

  </UContainer>
</template>
