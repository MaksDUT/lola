<script setup lang="ts">
const { data: page } = await useAsyncData('formules', () => queryCollection('formules').first())

const title = page.value?.seo?.title || page.value?.title
const description = page.value?.seo?.description || page.value?.description

useSeoMeta({
  title,
  ogTitle: title,
  description,
  ogDescription: description
})


const sections = ref([
  {
    title: 'Features',
    features: [
      {
        title: 'Durée',
        tiers: {
          'echappee': '2 à 4 jours',
          'grande-echappee': '5 à 7 jours',
          'horizon': '5 à 7 jours',
          'lointain-horizon': '5 à 7 jours',
          'aventure': '14 à 16 jours',
          'grande-aventure': '14 à 16 jours',
        }
      },
      {
        title: 'Destination',
        tiers: {
          'echappee': '1 lieu (France ou Europe)',
          'grande-echappee': '1 lieu (France ou Europe)',
          'horizon': '2 lieux (France ou Europe)',
          'lointain-horizon': '1 lieu (hors Europe)',
          'aventure': '1 ou 2 lieux (France ou Europe)',
          'grande-aventure': '1 ou 2 lieux (hors Europe)',
        }
      }
    ]
  },
])




</script>

<template>
  <div v-if="page">
    <UContainer>

      
        <div class="py-16 max-w-3xl mx-auto text-center mb-10">
          <h2 class="text-3xl font-bold mb-4">{{ title }}</h2>
          <p class="text-lg">{{ description }} </p>
        </div>

        <UPageGrid class="p-6">
          <UPageCard v-for="(card, index) in page.form.formules" spotlight spotlight-color="primary" :key="index" to=""
            class="p-0" :ui="{
              body: 'p-0',
              container: 'sm:p-0 border-round',
              root: 'p-0',

            }">

            <NuxtImg :src="card.image" width="1920" height="1080" :modifiers="{ fit: 'outside', rotate: '90' }"
              :custom="true" v-slot="{ src, isLoaded, imgAttrs }">

              <img v-if="isLoaded" v-bind="imgAttrs" :src="src">

            </NuxtImg>

            <div class="p-6">
              <h3 class="font-bold text-center text-xl text-[#a3b18a]">
                {{ card.title }}
              </h3>

              <h4 class="font-bold text-gray-500 text-center">
                {{ card.description }}
              </h4>

              <ul class="text-left list-disc list-inside text-sm p-3">
                <li class="list-none" v-for="feature in card.features">
                  {{ feature }}
                </li>
              </ul>

              <div class="flex justify-center">
                <UButton label="En savoir plus" class="text-center bg-lola-bg-1 text-black hover:bg-lola-bg-2" :to="card.link" />
              </div>

            </div>


          </UPageCard>
        </UPageGrid>

        <UPricingTable :tiers="page.form.formules.slice(0, -2)" :sections="sections" />

    </UContainer>

  </div>
</template>
