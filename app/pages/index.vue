<script setup lang="ts">
const { data: page } = await useAsyncData('index', () => queryCollection('index').first())

const title = page.value?.seo?.title || page.value?.title
const description = page.value?.seo?.description || page.value?.description

const valeurs = ref(page.value?.valeurs.valeurs)

useSeoMeta({
  titleTemplate: '',
  title,
  ogTitle: title,
  description,
  ogDescription: description
})
</script>

<template>
  <div v-if="page">

    <section class="relative h-screen w-full flex items-center justify-center">
      <!-- Background Video -->
      <video autoplay muted loop playsinline class="absolute top-0 left-0 w-full h-full object-cover">
        <source src="/video/background.mp4" type="video/mp4" />
      </video>

      <!-- Overlay -->
      <div class="absolute inset-0 bg-black/20"></div>

      <!-- Hero Content -->
      <div class="relative z-10 text-center text-white px-6">
        <h1 class="text-5xl font-bold mb-4">Près d'Ailleurs</h1>
        <p class="text-xl">On s'occupe de tout, sauf de vos valises</p>
      </div>
    </section>


    <UPageHero :title="page.title" :description="page.description" :links="page.hero.links">
      <template #top>
        <HeroBackground />
      </template>

      <template #title>
        <MDC :value="page.title" unwrap="p" />
      </template>
      <div class="flex flex-row space-x-5 justify-center">
        <div class="polaroid rotate-[0deg] w-1/3 ">
          <img :src="page.hero.lola.src" alt="page.lola1.image.alt"
            class=" rounded-lg shadow-2xl ring ring-default w-full h-100 object-cover" />
          <div class="caption">{{ page.hero.lola.titre }}</div>
        </div>

        <div class="polaroid rotate-[0deg] w-1/3 ">
          <img :src="page.hero.anthony.src" alt="page.lola1.image.alt"
            class=" rounded-lg shadow-2xl ring ring-default w-full h-100 object-cover" />
          <div class="caption">{{ page.hero.anthony.titre }}</div>
        </div>
      </div>
    </UPageHero>


    <UPageSection v-for="(section, index) in page.sections" :key="index" :title="section.title"
      :description="section.description" :orientation="section.orientation" :reverse="section.reverse"
      :features="section.features">

      <NuxtImg v-if="section.image" :src="section.image"></NuxtImg>
    </UPageSection>

    <div class="conteneur">
      <div :class="`circle ${valeur.flip ? 'flipped' : ''}`" v-for="(valeur, index) in valeurs"
        @click="() => valeur.flip = !valeur.flip">
        <div class="inner" @click="() => valeur.flip != valeur.flip"> <!-- L'intérieur qui tourne -->
          <div :class="`face  ${index % 2 ? 'front' : 'front2'}`">{{ valeur.title }}</div>
          <!-- Face avant avec un titre -->
          <div :class="`face  ${index % 2 ? 'back' : 'back2'}`">{{ valeur.description }}</div>
          <!-- Face arrière avec le texte -->
        </div>
      </div>
    </div>


    <UPageSection :title="page.features.title" :description="page.features.description">
      <UPageGrid>
        <UPageCard v-for="(item, index) in page.features.items" :key="index" v-bind="item" spotlight>

          <template #title>
            <div class="text-manu text-3xl">
              {{ item.title }}
            </div>
          </template>

        </UPageCard>
      </UPageGrid>
    </UPageSection>


    <USeparator />


    <UPageSection id="delais" 
      >

        <UTimeline orientation="horizontal" size="2xl" :default-value="page.delais.delais.length" :items="page.delais.delais" class="w-full" />

    </UPageSection>




    <UPageSection id="testimonials" :headline="page.testimonials.headline" :title="page.testimonials.title"
      :description="page.testimonials.description">
      <UPageColumns class="xl:columns-4">
        <UPageCard v-for="(testimonial, index) in page.testimonials.items" :key="index" variant="subtle"
          :description="testimonial.quote"
          :ui="{ description: 'before:content-[open-quote] after:content-[close-quote]' }">
          <template #footer>
            <UUser v-bind="testimonial.user" size="lg" />
          </template>
        </UPageCard>
      </UPageColumns>
    </UPageSection>

    <USeparator />

    <UPageCTA v-bind="page.cta" variant="naked" class="overflow-hidden">
      <LazyStarsBg />
    </UPageCTA>
  </div>
</template>

<style scoped>
.polaroid {
  background: #fff;
  padding: 10px 10px 30px 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  display: inline-block;
  transform: rotate(0deg);
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



.circle {
  /* Conteneur du rond */
  width: 200px;
  height: 200px;
  /* Taille du rond */
  border-radius: 50%;
  /* Rend la forme circulaire */
  perspective: 1000px;
  /* Ajoute un effet 3D */
  cursor: pointer;
  /* Change le curseur en main */
}

.inner {
  /* Partie intérieure qui va tourner */
  width: 100%;
  height: 100%;
  /* Remplit tout le cercle */
  position: relative;
  /* Positionne les faces en relatif */
  transform-style: preserve-3d;
  /* Garde l'effet 3D */
  transition: transform 0.6s;
  /* Animation de rotation */
  border-radius: 50%;
  /* Reste circulaire */
}

.circle.flipped .inner {
  /* Quand la classe "flipped" est ajoutée */
  transform: rotateY(180deg);
  /* Retourne l'élément en Y */
}

.conteneur {
  display: flex;
  /* aligne les enfants en ligne (row) */
  flex-direction: row;
  /* explicite : ligne */
  gap: 24px;
  /* espace entre les cercles */
  justify-content: center;
  /* centre horizontalement dans la zone parent */
  align-items: center;
  /* centre verticalement */
  flex-wrap: wrap;
  /* permet de passer à la ligne sur petits écrans */
  width: 100%;
  /* s'étend sur la largeur disponible */
}


.face {
  /* Style commun aux deux faces */
  position: absolute;
  inset: 0;
  /* Remplit tout le rond */
  display: flex;
  justify-content: center;
  align-items: center;
  /* Centre le texte */
  backface-visibility: hidden;
  /* Cache l'arrière quand on tourne */
  border-radius: 50%;

}

.front {
  /* Face avant */
  background: #A3B18A;
  color: #fff;
  font-family: 'Amatic SC', cursive;
  font-size: 2.5em;

}

.text-manu {
  color: #A3B18A;
  font-family: 'Amatic SC', cursive;
}

.back {
  /* Face arrière */
  background: #D8CFC4;
  color: #ffffff;
  transform: rotateY(180deg);
  /* Positionnée à l'envers */
  padding: 1rem;
  /* Espace autour du texte */
  text-align: center;
  font-family: 'Nuito', cursive;
  font-size: 1em;

}

.front2 {
  /* Face avant */
  background: #D8CFC4;
  color: #fff;
  font-family: 'Amatic SC', cursive;
  font-size: 2.5em;

}

.back2 {
  /* Face arrière */
  background: #A3B18A;
  color: #ffffff;
  transform: rotateY(180deg);
  /* Positionnée à l'envers */
  padding: 1rem;
  /* Espace autour du texte */
  text-align: center;
  font-family: 'Nuito', cursive;
  font-size: 1em;

}
</style>