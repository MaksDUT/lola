<script setup lang="ts">
const route = useRoute()

const { data: page } = await useAsyncData('index', () => queryCollection('index').first())

const { data: posts } = await useAsyncData(route.path, () => queryCollection('posts').limit(3).all())

const { data: data_formules } = await useAsyncData('formules', () => queryCollection('formules').first())


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

const { isMobile } = useDevice()

const timeline = isMobile ? 'vertical' : 'horizontal';


const anthony_carou = [{ src: "/img/apropos/antho1.jpg", caption: "Chypre - 2022 - Ville de Paphos" }, { src: "/img/apropos/antho1.jpg", caption: "Chypre - 2022 - Ville de Paphos" }, { src: "/img/apropos/antho2.jpg", caption: "Equateur - 2024 - Mirador de Indichuris" }, { src: "/img/apropos/antho3.jpg", caption: "Pays Bas - 2023 - Ville d'Amsterdam" }];



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
        <h1 class="text-9xl font-bold mb-4">Près d'Ailleurs</h1>
        <div class="typewriter">
          <h1 class="text-4xl">"On s'occupe de tout, sauf de vos valises"</h1>
        </div>
      </div>
    </section>


    <UPageHero :title="page.title" :description="page.description">
      <template #top>
        <HeroBackground />
      </template>

      <template #title>
        <MDC :value="page.title" unwrap="p" />
      </template>
      <div class="flex flex-col sm:flex-row space-y-15 sm:space-y-0 sm:space-x-30 justify-center">
        <div class="polaroid rotate-[0deg] sm:w-1/3 ">
          <img :src="page.hero.lola.src" alt="page.lola1.image.alt"
            class=" rounded-lg shadow-2xl ring ring-default w-full h-100 object-cover" />
          <div class="caption">{{ page.hero.lola.titre }}</div>
        </div>

        <div class="polaroid rotate-[0deg] sm:w-1/3 ">
          <img :src="page.hero.anthony.src" alt="page.lola1.image.alt"
            class=" rounded-lg shadow-2xl ring ring-default w-full h-100 object-cover" />
          <div class="caption">{{ page.hero.anthony.titre }}</div>
        </div>
      </div>


      <div class="flex justify-center">
        <UButton :label="page.hero.links[0]?.label" :size="page.hero.links[0]?.size" :color="page.hero.links[0]?.color"
          :icon="page.hero.links[0]?.icon" :trailing="page.hero.links[0]?.trailing" :target="page.hero.links[0]?.target"
          :to="page.hero.links[0]?.to"></UButton>
      </div>

    </UPageHero>

    <div class="separator text-primary opacity-25">
    </div>

    <UPageSection v-for="(section, index) in page.sections" :key="index" :title="section.title"
      :description="section.description" :orientation="section.orientation" :reverse="section.reverse"
      :features="section.features" :ui="{
        container: 'flex flex-col lg:grid py-16 sm:py-24 lg:py-16 gap-8 sm:gap-16'
      }">

      <NuxtImg v-if="section.image" :src="section.image"></NuxtImg>
    </UPageSection>

    <div class="separator text-primary opacity-25">
    </div>
    <UPageSection title="Nos valeurs" :ui="{
      container: 'w-full max-w-(--ui-container) mx-auto px-4 sm:px-6 lg:px-0 flex flex-col lg:grid py-16 sm:py-24 lg:py-32 gap-8 sm:gap-16'
    }">
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
    </UPageSection>


    <div class="separator text-primary opacity-25">
    </div>


    <UPageSection :title="page.features.title" :description="page.features.description">
      <UPageGrid class="lg:grid-cols-3">
        <UPageCard v-for="(item, index) in page.features.items" :key="index" v-bind="item" spotlight>

          <template #title>
            <div class="text-manu text-3xl">
              {{ item.title }}
            </div>
          </template>

        </UPageCard>
      </UPageGrid>
    </UPageSection>


    <div class="separator text-primary opacity-25">
    </div>


    <UPageSection id="delais" :title="page.delais.title">

      <UTimeline :orientation="timeline" size="2xl" :default-value="page.delais.delais.length"
        :items="page.delais.delais" class="w-full" />

    </UPageSection>

    <div class="separator text-primary opacity-25">
    </div>

    <UPageSection id="blog" title="Nos aventure">
      <UPageBody>
        <UBlogPosts>
          <UBlogPost v-for="(post, index) in posts" :key="index" :to="post.path" :title="post.title"
            :description="post.description_intro" :image="post.image"
            :date="new Date(post.date).toLocaleDateString('en', { year: 'numeric', month: 'short', day: 'numeric' })"
            :authors="post.authors" :badge="post.badge" variant="naked" :ui="{
              description: ''
            }" />
        </UBlogPosts>
      </UPageBody>

    </UPageSection>


    <div class="separator text-primary opacity-25">
    </div>

    <UPageSection id="formules" title="Découvrez nos formules"
      description="Voici une petit aperçu de l'ensemble de nos forumules. Découvrez les et choisissez celle qui vous convient le mieux !1">
      <UCarousel v-slot="{ item }" loop auto-scroll :items="data_formules.form.formules"
        :ui="{ item: 'basis-1/3 relative p-0 m-2' }">
        <!-- <img :src="item.image" width="234" height="234" class="rounded-lg w-full h-fit object-cover"> -->
        <NuxtImg :src="item.image" width="1920" height="1080" class="w-full h-80 object-cover"
          :modifiers="{ fit: 'outside', rotate: '90' }" :custom="true" v-slot="{ src, isLoaded, imgAttrs }">

          <img v-if="isLoaded" v-bind="imgAttrs" :src="src">

        </NuxtImg>
        <!-- overlay caption -->
        <div v-if="item.description"
          class="absolute bottom-0 left-0 w-full bg-gradient-to-t from-black/70 via-black/40 to-transparent text-white p-4 text-sm md:text-base rounded-lg">
          <NuxtLink class="font-semibold text-center" :to="item.link">
            {{ item.title }}
          </NuxtLink>

        </div>
      </UCarousel>
    </UPageSection>



    <div class="separator text-primary opacity-25">
    </div>


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

  </div>
</template>

<style scoped>
.polaroid {
  background: #fff;
  padding: 10px 10px 15px 10px;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.2);
  display: inline-block;
  transform: rotate(0deg);
  transition: transform 0.3s ease;
}

.polaroid:hover {
  transform: rotate(0deg) scale(1.05);
}

.caption {
  text-align: center;
  margin-top: 15px;
  font-size: 1.5rem;
  color: #555;
  font-style: italic;
  font-weight: 700;
}



.circle {
  /* Conteneur du rond */
  width: 190px;
  height: 190px;
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
  font-size: 2em;

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
  font-size: 2em;

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


/* ==== SEPARATION EN "...." ==== */
.separator {
  border: none;
  /* Supprime la bordure par défaut */
  border-top: 5px dotted #000;
  /* Bordure supérieure en pointillés (noire) */
  margin: 50px 100px;
  /* Espacement haut/bas */
}
</style>
