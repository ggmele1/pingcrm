<template>
  <div>
    <portal-target name="dropdown" slim />
    <nav class="bg-gray-800">
      <div
        class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8"
        @click="hideDropdownMenus"
      >
        <div class="flex items-center justify-between h-16">
          <!-- Desktop Menu Items -->
          <div class="flex items-center">
            <div class="flex-shrink-0">
              <inertia-link class="mt-1" href="/">
                <logo class="fill-white" width="120" height="28" />
              </inertia-link>
            </div>

            <div class="hidden md:block">
              <main-menu
                :url="url()"
                class="ml-10 flex items-baseline space-x-4"
              />
            </div>
          </div>

          <div class="md:flex md:flex-shrink-0">
            <div class="ml-4 flex items-center md:ml-6">
              <!-- Profile dropdown -->
              <div
                class="hidden lg:block mt-1 mr-4 text-gray-300 py-2 rounded-md text-sm font-medium"
              >
                {{ $page.auth.user.account.name }}
              </div>
              <dropdown class="hidden md:block" placement="bottom-end">
                <div class="flex items-center cursor-pointer select-none group">
                  <div
                    class="text-white group-hover:text-indigo-600 focus:text-indigo-600 mr-1 whitespace-no-wrap"
                  >
                    <span>{{ $page.auth.user.first_name }}</span>
                    <span class="hidden md:inline">{{
                      $page.auth.user.last_name
                    }}</span>
                  </div>
                  <icon
                    class="w-5 h-5 group-hover:fill-indigo-600 fill-white focus:fill-indigo-600"
                    name="cheveron-down"
                  />
                </div>
                <div
                  slot="dropdown"
                  class="mt-2 py-2 shadow-xl bg-white rounded text-sm"
                >
                  <inertia-link
                    class="block px-6 py-2 hover:bg-indigo-500 hover:text-white"
                    :href="route('users.edit', $page.auth.user.id)"
                    >My Profile</inertia-link
                  >
                  <inertia-link
                    class="block px-6 py-2 hover:bg-indigo-500 hover:text-white"
                    :href="route('users')"
                    >Manage Users</inertia-link
                  >
                  <inertia-link
                    class="block px-6 py-2 hover:bg-indigo-500 hover:text-white"
                    :href="route('logout')"
                    method="post"
                    >Logout</inertia-link
                  >
                </div>
              </dropdown>

              <!-- Mobile Drop Down START -->
              <dropdown class="md:hidden" placement="bottom-end">
                <svg
                  class="fill-white w-6 h-6"
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 20 20"
                >
                  <path d="M0 3h20v2H0V3zm0 6h20v2H0V9zm0 6h20v2H0v-2z" />
                </svg>
                <!-- Mobile Menu Content -->
                <div
                  slot="dropdown"
                  class="mt-2 px-8 py-2 shadow-lg bg-gray-800 rounded"
                >
                  <!-- Page Nav Options -->
                  <div class="px-2 pt-2 pb-3 space-y-1 sm:px-3">
                    <main-menu :url="url()" class="" />
                  </div>
                  <div class="pt-4 pb-3 border-t border-gray-700">
                    <div class="px-2 pt-2 pb-3 space-y-1 sm:px-3">
                      <div
                        class=" text-base font-medium leading-none text-white"
                      >
                        <span>{{ $page.auth.user.first_name }}</span>
                        <span>{{ $page.auth.user.last_name }}</span>
                      </div>
                      <div class="mr-4 text-white text-sm font-medium">
                        {{ $page.auth.user.account.name }}
                      </div>
                    </div>
                    <!-- Profile Dropdown Options -->
                    <div class="mr-2">
                      <inertia-link
                        class="block px-6 py-2 text-sm font-medium leading-none text-gray-400"
                        :href="route('users.edit', $page.auth.user.id)"
                        >My Profile</inertia-link
                      >
                      <inertia-link
                        class="block px-6 py-2 text-sm font-medium leading-none text-gray-400"
                        :href="route('users')"
                        >Manage Users</inertia-link
                      >
                      <inertia-link
                        class="block px-6 py-2 text-sm font-medium leading-none text-gray-400"
                        :href="route('logout')"
                        method="post"
                        >Logout</inertia-link
                      >
                    </div>
                  </div>
                </div>
              </dropdown>
            </div>
          </div>
        </div>
      </div>
    </nav>

    <!-- Page Content -->
    <main>
      <div class="max-w-7xl mx-auto py-6 sm:px-6 lg:px-8">
        <div class="px-4 py-6 sm:px-0">
          <flash-messages />
          <slot />
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import Dropdown from "@/Shared/Dropdown";
import FlashMessages from "@/Shared/FlashMessages";
import Icon from "@/Shared/Icon";
import Logo from "@/Shared/Logo";
import MainMenu from "@/Shared/MainMenu";

export default {
  components: {
    Dropdown,
    FlashMessages,
    Icon,
    Logo,
    MainMenu
  },
  data() {
    return {
      showUserMenu: false,
      accounts: null
    };
  },
  methods: {
    url() {
      return location.pathname.substr(1);
    },
    hideDropdownMenus() {
      this.showUserMenu = false;
    }
  }
};
</script>
