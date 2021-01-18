<template>
  <div>
    <inertia-link class="flex items-center group py-3" :href="route('dashboard')">
      <div :class="isUrl('')
        ? 'bg-gray-900 text-white px-3 py-2 rounded-md text-sm font-medium'
        : 'text-gray-300 hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md text-sm font-medium'
      "
      >
        Dashboard
      </div>
    </inertia-link>
    <inertia-link
      class="flex items-center group py-3"
      :href="route('organizations')"
    >
      <div
        :class="
          isUrl('organizations')
            ? 'bg-gray-900 text-white px-3 py-2 rounded-md text-sm font-medium'
            : 'text-gray-300 hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md text-sm font-medium'
        "
      >
        Organizations
      </div>
    </inertia-link>
    <inertia-link
      class="flex items-center group py-3"
      :href="route('contacts')"
    >
      <div
        :class="
          isUrl('contacts')
            ? 'bg-gray-900 text-white px-3 py-2 rounded-md text-sm font-medium'
            : 'text-gray-300 hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md text-sm font-medium'
        "
      >
        Contacts
      </div>
    </inertia-link>
    <inertia-link class="flex items-center group py-3" :href="route('reports')">
      <div
        :class="
          isUrl('reports')
            ? 'bg-gray-900 text-white px-3 py-2 rounded-md text-sm font-medium'
            : 'text-gray-300 hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md text-sm font-medium'
        "
      >
        Reports
      </div>
    </inertia-link>
  </div>
</template>

<script>
export default {
  props: {
    url: String,
  },
  methods: {
    isUrl(...urls) {
      if (urls[0] === '') {
        return this.url === ''
      }
      return urls.filter(url => this.url.startsWith(url)).length
    },
  },
}
</script>
