<script setup lang="ts">
const { data: page } = await useAsyncData('apropos', () => queryCollection('apropos').first())

const title = page.value?.seo?.title || page.value?.title
const description = page.value?.seo?.description || page.value?.description

const anthony_carou = [{ src: "/img/apropos/antho1.jpg", caption: "Chypre - 2022 - Ville de Paphos" }, { src: "/img/apropos/antho2.jpg", caption: "Equateur - 2024 - Mirador de Indichuris" }, { src: "/img/apropos/antho3.jpg", caption: "Pays Bas - 2023 - Ville d'Amsterdam" }];



const anthony_pays = ref([
  {
    image: {
      path: '/img/apropos/Flag_of_Ecuador.svg.png',
      width: 400,
      height: 300,
    },
    text: "Equateur",
    class: "p-0",
    orientation: 'vertical' as const
  },

  {
    image: {
      path: '/img/apropos/Flag_of_Egypt.svg.png',
      width: 400,
      height: 300,
    },
    text: "Egypte",
    class: "p-0",
    orientation: 'vertical' as const
  },

  {
    image: {
      path: '/img/apropos/Flag_of_Italy.svg.png',
      width: 400,
      height: 300,
    },
    text: "Italie",
    class: "p-0",
    orientation: 'vertical' as const
  },

])

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

    <UPageSection :title="page.title" :description="page.description">

      <UPageHero :title="page.lola1.title" :description="page.lola1.description" orientation="horizontal">
        <div class="polaroid rotate-[3deg]">
          <img :src="page.lola1.image.src" :alt="page.lola1.image.alt"
            class=" rounded-lg shadow-2xl ring ring-default" />
          <div class="caption">{{ page.lola1.image.alt }}</div>
        </div>
      </UPageHero>

      <UPageHero :description="page.lola2.description" orientation="horizontal" reverse>
        <div class="polaroid">
          <img :src="page.lola2.image.src" :alt="page.lola2.image.alt"
            class=" rounded-lg shadow-2xl ring ring-default" />
          <div class="caption">{{ page.lola2.image.alt }}</div>
        </div>
      </UPageHero>

      <UPageHero :description="page.lola3.description" orientation="horizontal">
        <div class="flex flex-col space-y-5">
          <div class="polaroid rotate-[-3deg]">
            <img :src="page.lola3.image1.src" :alt="page.lola3.image1.alt"
              class=" rounded-lg shadow-2xl ring ring-default " />
            <div class="caption">{{ page.lola3.image1.alt }}</div>
          </div>

          <div class="polaroid rotate-[3deg]">
            <img :src="page.lola3.image2.src" :alt="page.lola3.image2.alt"
              class=" rounded-lg shadow-2xl ring ring-default" />
            <div class="caption">{{ page.lola3.image1.alt }}</div>
          </div>
        </div>
      </UPageHero>

      <UPageHero :title="page.anthony.title" :description="page.anthony.description" orientation="vertical">
      </UPageHero>


      <UCarousel v-slot="{ item }" class-names arrows :items="page.anthony.diapo" class="w-full md:w-2/3 mx-auto" :ui="{
        item: 'transition-opacity [&:not(.is-snapped)]:opacity-10 relative rounded-lg'
      }">
        <img :src="item.src" class="object-cover w-full h-70 md:h-170 rounded-lg ">

        <!-- overlay caption -->
        <div v-if="item.alt"
          class="absolute bottom-0 left-0 w-full bg-gradient-to-t from-black/70 via-black/40 to-transparent text-white p-4 text-sm md:text-base rounded-lg">
          <p class="font-semibold text-center">{{ item.alt }}</p>
        </div>
      </UCarousel>

      <section class="">
        <div class="w-full bg-base-200 py-10 px-6">
          <div class="max-w-6xl mx-auto text-center">

            <p class="text-base md:text-lg lg:text-xl text-justify leading-relaxed max-w-5xl mx-auto">
              {{ page.anthony.text_apres_diapo_1 }}
            </p>

            <p class="text-base md:text-lg lg:text-xl text-justify leading-relaxed max-w-5xl mx-auto"><br> Parmis
              {{ page.anthony.text_apres_diapo_2 }} </p>
          </div>
        </div>
      </section>

      <UPageGrid>
        <UPageCard v-for="(card, index) in page.anthony.drapeaux" :key="index" v-bind="card">

          <img v-if="card.src" :src="card.src" />

          <p class="text-base md:text-lg lg:text-xl text-justif text-center">{{ card.alt }}</p>

        </UPageCard>
      </UPageGrid>

      <section class="">
        <div class="w-full bg-base-200 py-10 px-6">
          <div class="max-w-6xl mx-auto text-center">
            <p class="text-base md:text-lg lg:text-xl text-justify leading-relaxed max-w-5xl mx-auto">
              {{ page.anthony.text_apres_drapeau_1 }}
            </p>

            <p class="text-base md:text-lg lg:text-xl text-justify leading-relaxed max-w-5xl mx-auto"><br>
              {{ page.anthony.text_apres_drapeau_2 }} </p>
          </div>
        </div>
      </section>


      <UPageHero :description="page.anthony.lien.description" :links="[{
        label: page.anthony.lien.boutton,
        to: page.anthony.lien.lien,
        color: 'neutral',
        variant: 'subtle',
        trailingIcon: page.anthony.lien.icon
      }]" />


    </UPageSection>
  </div>
</template>


<style scoped>
.polaroid {
  background: #fff;
  padding: 10px 10px 30px 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  display: inline-block;
  transform: rotate(-3deg);
  transition: transform 0.3s ease;
}

.polaroid:hover {
  transform: rotate(0deg) scale(1.05);
}

.caption {
  text-align: center;
  margin-top: 8px;
  font-size: 0.9rem;
  color: #555;
  font-style: italic;
}
</style>