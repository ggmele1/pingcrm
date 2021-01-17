<template>
  <div>
    <h1 class="mb-8 font-bold text-3xl">
      <inertia-link
        class="text-indigo-400 hover:text-indigo-600"
        :href="route('contacts')"
        >Contacts</inertia-link
      >
      <span class="text-indigo-400 font-medium">/</span>
      {{ form.first_name }} {{ form.last_name }}
    </h1>
    <trashed-message v-if="contact.deleted_at" class="mb-6" @restore="restore">
      This contact has been deleted.
    </trashed-message>
    <div class="bg-white rounded shadow overflow-hidden max-w-3xl">
      <form @submit.prevent="submit">
        <div class="p-8 -mr-6 -mb-8 flex flex-wrap">
          <text-input
            v-model="form.first_name"
            :error="errors.first_name"
            class="pr-6 pb-8 w-full lg:w-1/2"
            label="First name"
          />
          <text-input
            v-model="form.last_name"
            :error="errors.last_name"
            class="pr-6 pb-8 w-full lg:w-1/2"
            label="Last name"
          />
          <select-input
            v-model="form.organization_id"
            :error="errors.organization_id"
            class="pr-6 pb-8 w-full lg:w-1/2"
            label="Organization"
          >
            <option :value="null" />
            <option
              v-for="organization in organizations"
              :key="organization.id"
              :value="organization.id"
              >{{ organization.name }}</option
            >
          </select-input>
          <text-input
            v-model="form.email"
            :error="errors.email"
            class="pr-6 pb-8 w-full lg:w-1/2"
            label="Email"
          />
          <text-input
            v-model="form.phone"
            :error="errors.phone"
            class="pr-6 pb-8 w-full lg:w-1/2"
            label="Phone"
          />
          <text-input
            v-model="form.address"
            :error="errors.address"
            class="pr-6 pb-8 w-full lg:w-1/2"
            label="Address"
          />
          <text-input
            v-model="form.city"
            :error="errors.city"
            class="pr-6 pb-8 w-full lg:w-1/2"
            label="City"
          />
          <text-input
            v-model="form.region"
            :error="errors.region"
            class="pr-6 pb-8 w-full lg:w-1/2"
            label="Province/State"
          />
          <select-input
            v-model="form.country"
            :error="errors.country"
            class="pr-6 pb-8 w-full lg:w-1/2"
            label="Country"
          >
            <option :value="null" />
            <option value="CA">Canada</option>
            <option value="US">United States</option>
          </select-input>
          <text-input
            v-model="form.postal_code"
            :error="errors.postal_code"
            class="pr-6 pb-8 w-full lg:w-1/2"
            label="Postal code"
          />
        </div>
        <div
          class="px-8 py-4 bg-gray-100 border-t border-gray-200 flex items-center"
        >
          <button
            v-if="!contact.deleted_at"
            class="text-red-600 hover:underline"
            tabindex="-1"
            type="button"
            @click="showModal = true"
          >
            Delete Contact
          </button>
          <loading-button
            :loading="sending"
            class="btn-indigo ml-auto"
            type="submit"
            >Update Contact</loading-button
          >
        </div>
        <!-- Confirm Edit Modal  -->
        <div v-if="showModal">
          <div class="fixed z-10 inset-0 overflow-y-auto">
            <div
              class="flex items-end justify-center min-h-screen pt-4 px-4 pb-20 text-center sm:block sm:p-0"
            >
              <div class="fixed inset-0 transition-opacity" aria-hidden="true">
                <div class="absolute inset-0 bg-gray-500 opacity-75"></div>
              </div>

              <span
                class="hidden sm:inline-block sm:align-middle sm:h-screen"
                aria-hidden="true"
                >&#8203;</span
              >

              <div
                class="inline-block align-bottom bg-white rounded-lg text-left overflow-hidden shadow-xl transform transition-all sm:my-8 sm:align-middle sm:max-w-lg sm:w-full"
                role="dialog"
                aria-modal="true"
                aria-labelledby="modal-headline"
              >
                <div class="bg-white px-4 pt-5 pb-4 sm:p-6 sm:pb-4">
                  <div class="sm:flex sm:items-start">
                    <div
                      class="mx-auto flex-shrink-0 flex items-center justify-center h-12 w-12 rounded-full bg-red-100 sm:mx-0 sm:h-10 sm:w-10"
                    >
                      <svg
                        class="h-6 w-6 text-red-600"
                        xmlns="http://www.w3.org/2000/svg"
                        fill="none"
                        viewBox="0 0 24 24"
                        stroke="currentColor"
                        aria-hidden="true"
                      >
                        <path
                          stroke-linecap="round"
                          stroke-linejoin="round"
                          stroke-width="2"
                          d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z"
                        />
                      </svg>
                    </div>
                    <div class="mt-3 text-center sm:mt-0 sm:ml-4 sm:text-left">
                      <h3
                        class="text-lg leading-6 font-medium text-gray-900"
                        id="modal-headline"
                      >
                        Delete Contact
                      </h3>
                      <div class="mt-2">
                        <p class="text-sm text-gray-500">
                          Are you sure you want to delete this contact?
                        </p>
                      </div>
                    </div>
                  </div>
                </div>
                <div
                  class="bg-gray-50 px-4 py-3 sm:px-6 sm:flex sm:flex-row-reverse"
                >
                  <button
                    type="button"
                    class="w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 bg-red-600 text-base font-medium text-white hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-red-500 sm:ml-3 sm:w-auto sm:text-sm"
                    @click="destroy"
                  >
                    Delete
                  </button>
                  <button
                    type="button"
                    class="mt-3 w-full inline-flex justify-center rounded-md border border-gray-300 shadow-sm px-4 py-2 bg-white text-base font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 sm:mt-0 sm:ml-3 sm:w-auto sm:text-sm"
                    @click="showModal = false"
                  >
                    Cancel
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import Layout from "@/Shared/Layout";
import LoadingButton from "@/Shared/LoadingButton";
import SelectInput from "@/Shared/SelectInput";
import TextInput from "@/Shared/TextInput";
import TrashedMessage from "@/Shared/TrashedMessage";

export default {
  metaInfo() {
    return {
      title: `${this.form.first_name} ${this.form.last_name}`
    };
  },
  layout: Layout,
  components: {
    LoadingButton,
    SelectInput,
    TextInput,
    TrashedMessage
  },
  props: {
    errors: Object,
    contact: Object,
    organizations: Array
  },
  remember: "form",
  data() {
    return {
      showModal: false,
      sending: false,
      form: {
        first_name: this.contact.first_name,
        last_name: this.contact.last_name,
        organization_id: this.contact.organization_id,
        email: this.contact.email,
        phone: this.contact.phone,
        address: this.contact.address,
        city: this.contact.city,
        region: this.contact.region,
        country: this.contact.country,
        postal_code: this.contact.postal_code
      }
    };
  },
  methods: {
    submit() {
      this.$inertia.put(
        this.route("contacts.update", this.contact.id),
        this.form,
        {
          onStart: () => (this.sending = true),
          onFinish: () => (this.sending = false)
        }
      );
    },
    destroy() {
      this.$inertia.delete(this.route("contacts.destroy", this.contact.id));
      this.showModal = false;
    },
    restore() {
      if (confirm("Are you sure you want to restore this contact?")) {
        this.$inertia.put(this.route("contacts.restore", this.contact.id));
      }
    }
  }
};
</script>
