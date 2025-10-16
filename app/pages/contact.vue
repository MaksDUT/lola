<script setup lang="ts">

import type { FormError, FormSubmitEvent } from '@nuxt/ui'


const state = reactive({
  nom: undefined,
  prenom: undefined,
  message: undefined,
  email: undefined
})
const toast = useToast()

async function onSubmit(event: FormSubmitEvent<typeof state>) {
  toast.add({ title: 'Success', description: 'formulaire envoyé', color: 'success' })
  console.log(event.data)

}
</script>

<template>

  <!-- Contact Form -->
  <section id="contact" class="py-16 bg-base-200">
    <div class="max-w-3xl mx-auto text-center mb-10">
      <h2 class="text-3xl font-bold mb-4">Contactez-nous</h2>
      <p class="text-lg">Remplissez le formulaire ci-dessous et nous vous répondrons rapidement.</p>
    </div>

    <div class="max-w-2xl mx-auto bg-white rounded-lg p-8">


      <UCard>
        <form class="space-y-4" action="https://formsubmit.co/58ee976116bb0fe50bf54efc20dd8976" method="POST">
          <div class="field padding-bottom--24">
            <input type="text" placeholder="Votre nom" class="input input-bordered w-full bg-white" required
              name="nom" />
          </div>
          <div class="field padding-bottom--24">
            <input type="text" placeholder="Votre prénom" class="input input-bordered w-full bg-white" required
              name="prenom" />
          </div>
          <div class="field padding-bottom--24">
            <input type="email" placeholder="Votre email" class="input input-bordered w-full bg-white" required
              name="email" />
          </div>
          <div class="field padding-bottom--24">
            <textarea class="textarea textarea-bordered w-full bg-white" placeholder="Votre message" rows="4" required
              name="message"></textarea>
          </div>

          <input type="hidden" name="_next" value="https://presdailleurs.fr/merci.html">
          <input type="hidden" name="_subject" value="Nouveau message de contact !">

          <button type="submit" class="btn bg-base-100 w-full md:w-auto hover:bg-grlola rounded p-2">Envoyez votre message</button>
        </form>
      </UCard>


    </div>
  </section>
</template>


<style scoped>
.field input, .field textarea {
  font-size: 16px;
  line-height: 28px;
  padding: 8px 16px;
  width: 100%;
  min-height: 44px;
  border: unset;
  border-radius: 4px;
  outline-color: rgb(84 105 212 / 0.5);
  background-color: rgb(255, 255, 255);
  box-shadow: rgba(0, 0, 0, 0) 0px 0px 0px 0px,
    rgba(0, 0, 0, 0) 0px 0px 0px 0px,
    rgba(0, 0, 0, 0) 0px 0px 0px 0px,
    rgba(60, 66, 87, 0.16) 0px 0px 0px 1px,
    rgba(0, 0, 0, 0) 0px 0px 0px 0px,
    rgba(0, 0, 0, 0) 0px 0px 0px 0px,
    rgba(0, 0, 0, 0) 0px 0px 0px 0px;
}

button[type="submit"] {
  background-color: #d8cfc4;
  color: oklch(37.2% 0.044 257.287);
  font-weight: 500;
  cursor: pointer;
}

.btn:hover{
  background-color: #f9fafb;
}
</style>