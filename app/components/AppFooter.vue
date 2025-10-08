<script setup lang="ts">
const columns = [{
  label: 'Resources',
  children: [{
    label: 'Help center'
  }, {
    label: 'Docs'
  }, {
    label: 'Roadmap'
  }, {
    label: 'Changelog'
  }]
}, {
  label: 'Features',
  children: [{
    label: 'Affiliates'
  }, {
    label: 'Portal'
  }, {
    label: 'Jobs'
  }, {
    label: 'Sponsors'
  }]
}, {
  label: 'Company',
  children: [{
    label: 'About'
  }, {
    label: 'Pricing'
  }, {
    label: 'Careers'
  }, {
    label: 'Blog'
  }]
}]

const toast = useToast()

const email = ref('')
const loading = ref(false)

function onSubmit() {
  loading.value = true

  toast.add({
    title: 'Subscribed!',
    description: 'You\'ve been subscribed to our newsletter.'
  })
}
</script>

<template>
  <USeparator
    class="h-px"
  />

  <UFooter :ui="{ top: 'border-b border-default' }">
   <!--  <template #top>
      <UContainer>
        <UFooterColumns :columns="columns">
          <template #right>
            <form @submit.prevent="onSubmit">
              <UFormField
                name="email"
                label="Subscribe to our newsletter"
                size="lg"
              >
                <UInput
                  v-model="email"
                  type="email"
                  class="w-full"
                  placeholder="Enter your email"
                >
                  <template #trailing>
                    <UButton
                      type="submit"
                      size="xs"
                      color="neutral"
                      label="Subscribe"
                    />
                  </template>
                </UInput>
              </UFormField>
            </form>
          </template>
        </UFooterColumns>
      </UContainer>
    </template> -->

    <template #left>
      <p class="text-muted text-sm">
        Près d'ailleurs • © {{ new Date().getFullYear() }} - Tous droits réservés
      </p>
    </template>

    <template #right>
      <UButton
        to="https://www.facebook.com/share/1BBPqMHara/"
        target="_blank"
        icon="i-simple-icons-facebook"
        aria-label="Facebook"
        color="neutral"
        variant="ghost"
      />
      <UButton
        to="https://www.instagram.com/pres.dailleurs"
        target="_blank"
        icon="i-simple-icons-instagram"
        aria-label="Nuxt on X"
        color="neutral"
        variant="ghost"
      />
      <!-- <UButton
        to="https://github.com/nuxt-ui-templates/saas"
        target="_blank"
        icon="i-simple-icons-linkedin"
        aria-label="Nuxt UI on GitHub"
        color="neutral"
        variant="ghost"
      /> -->
    </template>
  </UFooter>
</template>
