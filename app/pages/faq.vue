<script setup lang="ts">
const { data: page } = await useAsyncData('faq', () => queryCollection('faq').first())

const title = page.value?.seo?.title || page.value?.title
const description = page.value?.seo?.description || page.value?.description

useSeoMeta({
  title,
  ogTitle: title,
  description,
  ogDescription: description
})

defineOgImageComponent('Saas')

</script>

<template>
  <div v-if="page">

    <div class="pt-16 max-w-3xl mx-auto text-center mb-10">
      <h2 class="text-3xl font-bold mb-4">{{ page.faq.title }}</h2>
      <p class="text-lg">{{ page.faq.description }}</p>
    </div>
    <UPageSection :ui="{
      container: 'w-full max-w-(--ui-container) mx-auto px-4 sm:px-6 lg:px-8 flex flex-col lg:grid py-8 sm:py-8 lg:py-8 gap-8 sm:gap-16',
    }">

      <UAccordion :items="page.faq.items" :unmount-on-hide="false" :default-value="['0']" type="multiple"
        class="max-w-3xl mx-auto" :ui="{
          
          trigger: 'text-base text-highlighted',
          body: 'text-base text-muted'
        }"
        
        >
        <template #body="{ item }">
          <div v-html="item.content">
          </div>
        </template>
      </UAccordion>

    </UPageSection>
  </div>
</template>
