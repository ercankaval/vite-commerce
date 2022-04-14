<template>
  <div class="relative z-50">
    <button
        @click="toggleModal()"
        class="text-md font-general-medium bg-indigo-500 hover:bg-indigo-600 text-white shadow-sm rounded-md px-5 py-2.5 duration-300 flex"
    >
      <img src="/img/basket-icon.svg" class="h-5 mr-3"> Sepetimdeki Ürünler
    </button>
    <div v-if="modal"
         className="overflow-x-hidden overflow-y-auto fixed inset-0 z-50 outline-none focus:outline-none justify-center items-center flex">
      <div className="relative w-auto my-6 mx-auto max-w-6xl">
        <!--content-->
        <div
            className="border-0 rounded-lg shadow-lg relative flex flex-col w-full bg-white outline-none focus:outline-none">
          <!--header-->
          <div className="flex items-start justify-between p-5 border-b border-solid border-slate-200 rounded-t">
            <h3 className="text-3xl font-semibold">
              Modal Title
            </h3>
            <button
                className="p-1 ml-auto bg-transparent border-0 text-black opacity-5 float-right text-3xl leading-none font-semibold outline-none focus:outline-none"
                v-on:click="toggleModal()">
              <span
                  className="bg-transparent text-black opacity-5 h-6 w-6 text-2xl block outline-none focus:outline-none">
                ×
              </span>
            </button>
          </div>
          <!--body-->
          <div className="relative p-6 flex-auto">
            <p className="my-4 text-slate-500 text-lg leading-relaxed">
              I always felt like I could do anything. That’s the main
              thing people are controlled by! Thoughts- their perception
              of themselves! They're slowed down by their perception of
              themselves. If you're taught you can’t do anything, you
              won’t do anything. I was taught I could do everything.
            </p>
          </div>
          <!--footer-->
          <div className="flex items-center justify-end p-6 border-t border-solid border-slate-200 rounded-b">
            <button
                className="text-red-500 bg-transparent border border-solid border-red-500 hover:bg-red-500 hover:text-white active:bg-red-600 font-bold uppercase text-sm px-6 py-3 rounded outline-none focus:outline-none mr-1 mb-1 ease-linear transition-all duration-150"
                type="button" v-on:click="toggleModal()">
              Close
            </button>
            <button
                className="text-red-500 background-transparent font-bold uppercase px-6 py-2 text-sm outline-none focus:outline-none mr-1 mb-1 ease-linear transition-all duration-150"
                type="button" v-on:click="toggleModal()">
              Save Changes
            </button>
          </div>
        </div>
      </div>
    </div>
    <div v-if="modal" className="opacity-25 fixed inset-0 z-40 bg-black"></div>
  </div>
</template>

<script>
export default {
  name: "large-modal",
  data() {
    return {
      modal: false
    }
  },
  methods: {
    toggleModal: function () {
      this.modal = !this.modal;
    }
  }
}
</script>