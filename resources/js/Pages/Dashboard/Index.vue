<template>
  <div>
    <FirstLoginBanner v-if="parseInt($page.auth.user.is_first_login) < 1" :user="$page.auth.user" class="mb-8" />
    <div>
      <h1 class="mb-8 font-bold text-3xl">Dashboard</h1>
      <p class="mb-8 leading-normal">
        Hey there! Welcome to Ping CRM, a demo app designed to help illustrate
        how
        <a
          class="text-indigo-500 underline hover:text-orange-600"
          href="https://inertiajs.com"
        >Inertia.js</a>
        works.
      </p>
      <div class="mb-8 flex">
        <inertia-link class="btn-indigo" href="/500">500 error</inertia-link>
        <inertia-link class="btn-indigo ml-1" href="/404">
          404 error
        </inertia-link>
      </div>
      <p class="leading-normal">
        👆 These links are intended to be broken to illustrate how error
        handling works with Inertia.js.
      </p>
    </div>
  </div>
</template>

<script>
import Layout from '@/Shared/Layout'
import FirstLoginBanner from '@/Shared/FirstLoginBanner'

export default {
  metaInfo: { title: 'Dashboard' },
  layout: Layout,
  components: {
    FirstLoginBanner,
  },
  props: {
    user: Object,
  },

}
</script>
