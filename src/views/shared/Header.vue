<template>
  <nav id="nav" class="sm:container sm:mx-auto">
    <!-- Header start -->
    <div
        class="block my-3 sm:flex sm:items-center sm:justify-between z-10"
    >
      <!-- Header menu links and small screen hamburger menu -->
      <div class="flex justify-between items-center px-4 sm:px-0">
        <!-- Header logos -->
        <div>
          <router-link to="/"
          ><img
              src="/img/logo.svg"
              class="w-36"
              alt="Dark Logo"
          />
          </router-link>
        </div>


        <!-- Small screen hamburger menu -->
        <div class="sm:hidden">
          <button
              @click="isOpen = !isOpen"
              type="button"
              class="focus:outline-none"
              aria-label="Hamburger Menu"
          >
            <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                class="h-7 w-7 fill-current text-secondary-dark dark:text-ternary-light"
            >
              <path
                  v-if="isOpen"
                  fill-rule="evenodd"
                  d="M18.278 16.864a1 1 0 0 1-1.414 1.414l-4.829-4.828-4.828 4.828a1 1 0 0 1-1.414-1.414l4.828-4.829-4.828-4.828a1 1 0 0 1 1.414-1.414l4.829 4.828 4.828-4.828a1 1 0 1 1 1.414 1.414l-4.828 4.829 4.828 4.828z"
                  clip-rule="evenodd"
              ></path>
              <path
                  v-if="!isOpen"
                  fill-rule="evenodd"
                  d="M4 5h16a1 1 0 0 1 0 2H4a1 1 0 1 1 0-2zm0 6h16a1 1 0 0 1 0 2H4a1 1 0 0 1 0-2zm0 6h16a1 1 0 0 1 0 2H4a1 1 0 0 1 0-2z"
              ></path>
            </svg>
          </button>
        </div>
      </div>

      <!-- Header links -->
      <AppHeaderLinks />

      <!-- Header right section buttons -->
      <div
          class="hidden sm:flex justify-between items-center flex-col md:flex-row"
      >
        <!-- Hire me button -->
        <div class="hidden md:block">
          <BasketModal />
        </div>

      </div>
    </div>

  </nav>
</template>

<script>
import AppHeaderLinks from '@/views/shared/AppHeaderLinks.vue';
import BasketModal from '@/views/shared/BasketModal.vue';

export default {
  components: {AppHeaderLinks, BasketModal},
  data() {
    return {
      isOpen: false,
      theme: '',
      modal: true,
      categories: [
        {
          id: 1,
          value: 'web',
          name: 'Web Application',
        },
        {
          id: 2,
          value: 'mobile',
          name: 'Mobile Application',
        },
        {
          id: 3,
          value: 'ui-ux',
          name: 'UI/UX Design',
        },
        {
          id: 4,
          value: 'branding',
          name: 'Branding & Anim',
        },
      ]
    };
  },
  methods: {
    showModal() {
      if (this.modal) {
        // Stop screen scrolling
        document
            .getElementsByTagName('html')[0]
            .classList.remove('overflow-y-hidden');
        this.modal = false;
      } else {
        document
            .getElementsByTagName('html')[0]
            .classList.add('overflow-y-hidden');
        this.modal = true;
      }
    }
  }
}
</script>