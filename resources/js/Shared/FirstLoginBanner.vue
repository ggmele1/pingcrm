<template>
  <div class="bg-indigo-600">
    <div class="flex flex-row flex-wrap items-center rounded w-full justify-center p-2">
      <svg class="h-6 w-6 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden="true">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M11 5.882V19.24a1.76 1.76 0 01-3.417.592l-2.147-6.15M18 13a3 3 0 100-6M5.436 13.683A4.001 4.001 0 017 6h1.832c4.1 0 7.625-1.234 9.168-3v14c-1.543-1.766-5.067-3-9.168-3H7a3.988 3.988 0 01-1.564-.317z" />
      </svg>
      <p class="p-2 text-white text-base font-bold">Welcome! Please upload your photo!</p>
      <form class="flex flex-row flex-wrap items-center lg:w-1/2 sm:w-full m-2" @submit.prevent="submit">
        <FileInput v-model="form.photo" class="flex-grow m-2" type="file" accept="image/*" />
        <loading-button :loading="sending" class="btn-indigo m-3 border border-indigo hover:border-transparent" type="submit">Update User</loading-button>
      </form>
    </div>
  </div>
</template>

<script>
import FileInput from '@/Shared/FileInput'
import LoadingButton from '@/Shared/LoadingButton'
import Layout from '@/Shared/Layout'

export default {
  layout: Layout,
  components: {
    FileInput,
    LoadingButton,
  },
  props: {
    user: Object,
  },
  remember: 'form',
  data() {
    return {
      sending: false,
      form: {
        first_name: this.user.first_name,
        last_name: this.user.last_name,
        email: this.user.email,
        owner: this.user.owner,
        photo: null,
        is_first_login: this.user.is_first_login,
      },
    }
  },

  methods: {
    submit() {
      var data = new FormData()
      data.append('first_name', this.form.first_name || '')
      data.append('last_name', this.form.last_name || '')
      data.append('email', this.form.email || '')
      data.append('owner', this.form.owner ? '1' : '0')
      data.append('photo', this.form.photo || '')
      data.append('is_first_login', parseInt(this.form.is_first_login) + 1)
      data.append('_method', 'put')

      if (this.form.photo) {
        this.$inertia.post(this.route('users.update', this.user.id), data, {
          onStart: () => (this.sending = true),
          onFinish: () => (this.sending = false),
          onSuccess: () => {
            if (Object.keys(this.$page.errors).length === 0) {
              this.form.photo = null
              this.form.password = null
            }
          },
        })
      }


    },
  },
}
</script>