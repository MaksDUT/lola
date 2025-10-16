<script setup lang="ts">
import { onMounted } from 'vue'

const { data: page } = await useAsyncData('rdv', () => queryCollection('rdv').first())

const title = page.value?.seo?.title || page.value?.title
const description = page.value?.seo?.description || page.value?.description

useSeoMeta({
  title,
  ogTitle: title,
  description,
  ogDescription: description
})

defineOgImageComponent('Saas')


onMounted(() => {

  (function (C, A, L) { let p = function (a, ar) { a.q.push(ar); }; let d = C.document; C.Cal = C.Cal || function () { let cal = C.Cal; let ar = arguments; if (!cal.loaded) { cal.ns = {}; cal.q = cal.q || []; d.head.appendChild(d.createElement("script")).src = A; cal.loaded = true; } if (ar[0] === L) { const api = function () { p(api, arguments); }; const namespace = ar[1]; api.q = api.q || []; if (typeof namespace === "string") { cal.ns[namespace] = cal.ns[namespace] || api; p(cal.ns[namespace], ar); p(cal, ["initNamespace", namespace]); } else p(cal, ar); return; } p(cal, ar); }; })(window, "https://app.cal.com/embed/embed.js", "init");
  Cal("init", "15min", { origin: "https://app.cal.com" });

  Cal.ns["15min"]("inline", {
    elementOrSelector: "#my-cal-inline-15min",
    config: { "layout": "month_view", "theme": "light" },
    calLink: page.value.rdv.link_cal,
  });

  Cal.ns["15min"]("ui", { "theme": "light", "hideEventTypeDetails": true, "layout": "month_view" });

})
</script>

<template>
  <!-- Contact Form -->
  <section id="rdv" class="py-16 bg-base-200">
    <div class="max-w-3xl mx-auto text-center mb-10">
      <h2 class="text-3xl font-bold mb-4">{{ page.rdv.title }} </h2>
      <h2 class="text-3xl font-bold mb-4">{{ page.rdv.subtitle }}</h2>
      <p class="text-lg">{{ page.rdv.description }}</p>
    </div>
  </section>

  <div style="width:100%;height:100%;overflow:scroll" id="my-cal-inline-15min"></div>


</template>
