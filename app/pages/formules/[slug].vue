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

const link = post.value.formule.link_cal;
onMounted(() => {

  (function (C, A, L) { let p = function (a, ar) { a.q.push(ar); }; let d = C.document; C.Cal = C.Cal || function () { let cal = C.Cal; let ar = arguments; if (!cal.loaded) { cal.ns = {}; cal.q = cal.q || []; d.head.appendChild(d.createElement("script")).src = A; cal.loaded = true; } if (ar[0] === L) { const api = function () { p(api, arguments); }; const namespace = ar[1]; api.q = api.q || []; if (typeof namespace === "string") { cal.ns[namespace] = cal.ns[namespace] || api; p(cal.ns[namespace], ar); p(cal, ["initNamespace", namespace]); } else p(cal, ar); return; } p(cal, ar); }; })(window, "https://app.cal.com/embed/embed.js", "init");
  Cal("init", "15min", { origin: "https://app.cal.com" });

  Cal.ns["15min"]("floatingButton", { "calLink": link, "config": { "layout": "month_view" }, "buttonPosition": "bottom-right", "buttonText": "Prendre rendez-vous" });
  Cal.ns["15min"]("ui", { "hideEventTypeDetails": false, "layout": "month_view" });
})


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
  <UContainer v-if="post" class="flex flex-col justify-center min-h-[80vh]">

    <UPricingPlan :title="post.formule.title" :description="post.formule.description" :price="post.formule.price"
      billing-cycle=" TTC"  :ui="{
        title:'text-highlighted text-2xl sm:text-3xl md:text-4xl text-pretty font-semibold',
        titleWrapper: 'flex items-center gap-3 justify-center text-3xl',
        description: 'text-muted text-base text-pretty mt-8 text-justify'
      }">


      <template #features>
        <div class="flex flex-col md:flex-row justify-evenly gap-5 mt-5">
          <div>
            <h4 class="text-muted text-md font-bold pb-7">Inclus </h4>
            <ul class="flex flex-col gap-3 flex-1 grow-0">
              <li class="flex items-center gap-2 min-w-0" v-for="value in post.formule.features">
                <svg xmlns="http://www.w3.org/2000/svg" class="size-5 shrink-0 text-primary" width="24" height="24"
                  viewBox="0 0 24 24">
                  <g fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2">
                    <circle cx="12" cy="12" r="10" />
                    <path d="m9 12l2 2l4-4" />
                  </g>
                </svg>
                <span class="text-muted text-sm ">{{ value }}
                </span>
              </li>
            </ul>
          </div>
          <div>


            <h4 class="text-muted text-md font-bold pb-7">Non inclus </h4>

            <ul class="flex flex-col gap-3 flex-1 grow-0">
              <li class="flex items-center gap-2 min-w-0" v-for="value in post.formule.no_features">

                <svg xmlns="http://www.w3.org/2000/svg" class="size-5 shrink-0 text-error" width="24" height="24"
                  viewBox="0 0 24 24">
                  <g fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2">
                    <circle cx="12" cy="12" r="10" />
                    <path d="m15 9l-6 6m0-6l6 6" />
                  </g>
                </svg>

                <span class="text-muted text-sm  text-error">{{ value }}
                </span>
              </li>
            </ul>
          </div>
        </div>
      </template>

    </UPricingPlan>



  </UContainer>
</template>
