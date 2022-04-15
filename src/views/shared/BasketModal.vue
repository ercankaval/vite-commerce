<script>
export default {
  name: "large-modal",
  data() {
    return {
      modal: false,
      cartItems: [],
      showMessage: false,
      basketTotalItems: 0
    }
  },
  created() {
    this.cartItems = JSON.parse(window.localStorage.getItem('cartItems'))
    this.sumTotal()
  },
  methods: {
    toggleModal: function () {
      this.modal = !this.modal;
    },
    formatPrice(value) {
      let val = (value/1).toFixed(2).replace('.', ',')
      return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".")
    },
    totalPrice(price,qty) {
      const value = price * qty
      let val = (value/1).toFixed(2).replace('.', ',')
      return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".")
    },
    removeItem(index) {
      this.cartItems.splice(this.cartItems.indexOf(index), 1)
      window.localStorage.setItem('cartItems', JSON.stringify(this.cartItems))
      this.showMessage = true
      setTimeout(() => this.hideShowMessage(), 1000)
    },
    hideShowMessage() {
      this.showMessage = false
    },
    minusQuantity(itemToAdd) {
      const itemInCart = this.cartItems.filter(item => item.id===itemToAdd.id)
      const itemQty = itemToAdd.qty

      if (itemQty !== 1) {
        itemInCart[0].qty --;
        window.localStorage.setItem('cartItems', JSON.stringify(this.cartItems))
        this.showMessage = true
        setTimeout(() => this.hideShowMessage(), 1000)
        this.sumTotal()
      }
    },
    plusQuantity(itemToAdd) {
      const itemInCart = this.cartItems.filter(item => item.id===itemToAdd.id)

      itemInCart[0].qty ++;

      window.localStorage.setItem('cartItems', JSON.stringify(this.cartItems))
      this.showMessage = true
      setTimeout(() => this.hideShowMessage(), 1000)
      this.sumTotal()
    },
    sumTotal() {
      this.basketTotalItems = 0
      this.cartItems.forEach(val => {
        this.basketTotalItems += val.qty;
      });
    }
  }
}
</script>

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
            <h3 className="text-2xl font-semibold flex justify-between w-full">
              <span>Sepetimdeki Ürünler</span>
              <span>{{basketTotalItems}} Adet</span>
            </h3>
          </div>
          <!--body-->
          <div className="relative p-6">

            <div v-if="showMessage" class="bg-green-100 border border-green-400 text-green px-4 py-3 rounded relative mb-5" role="alert">
              <span class="block sm:inline">Sepetiniz güncellenmiştir.</span>
              <span @click="hideShowMessage()" class="absolute top-0 bottom-0 right-0 px-4 py-3">
                <svg class="fill-current h-6 w-6 text-red-500" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><title>Close</title><path d="M14.348 14.849a1.2 1.2 0 0 1-1.697 0L10 11.819l-2.651 3.029a1.2 1.2 0 1 1-1.697-1.697l2.758-3.15-2.759-3.152a1.2 1.2 0 1 1 1.697-1.697L10 8.183l2.651-3.031a1.2 1.2 0 1 1 1.697 1.697l-2.758 3.152 2.758 3.15a1.2 1.2 0 0 1 0 1.698z"/></svg>
              </span>
            </div>

            <div class="flex mb-5">
              <h3 class="font-semibold text-gray-600 text-xs uppercase w-2/5">Ürün Detayı</h3>
              <h3 class="font-semibold text-center text-gray-600 text-xs uppercase w-1/5 text-center">Adet</h3>
              <h3 class="font-semibold text-center text-gray-600 text-xs uppercase w-1/5 text-center">Fiyat</h3>
              <h3 class="font-semibold text-center text-gray-600 text-xs uppercase w-1/5 text-center">Toplam</h3>
            </div>

            <div
                class="flex items-center hover:bg-gray-100 px-6 py-5"
                v-for="item in cartItems"
                :key="item.id"
                :if="cartItems"
            >
              <div class="flex w-2/5"> <!-- product -->
                <div class="w-20">
                  <img class="w-full" :src="item.image" :alt="item.title">
                </div>
                <div class="flex flex-col justify-between ml-4 flex-grow">
                  <span class="font-bold text-sm">{{ item.title }}</span>
                  <a @click="removeItem(item)" class="font-semibold hover:text-red-500 text-gray-500 text-xs">Sil</a>
                </div>
              </div>
              <div class="flex justify-center w-1/5">
                <svg @click="minusQuantity(item)" class="fill-current text-gray-600 w-3" viewBox="0 0 448 512"><path d="M416 208H32c-17.67 0-32 14.33-32 32v32c0 17.67 14.33 32 32 32h384c17.67 0 32-14.33 32-32v-32c0-17.67-14.33-32-32-32z"/>
                </svg>

                <input class="mx-2 border text-center w-8" type="text" :value="item.qty">

                <svg @click="plusQuantity(item)" class="fill-current text-gray-600 w-3" viewBox="0 0 448 512">
                  <path d="M416 208H272V64c0-17.67-14.33-32-32-32h-32c-17.67 0-32 14.33-32 32v144H32c-17.67 0-32 14.33-32 32v32c0 17.67 14.33 32 32 32h144v144c0 17.67 14.33 32 32 32h32c17.67 0 32-14.33 32-32V304h144c17.67 0 32-14.33 32-32v-32c0-17.67-14.33-32-32-32z"/>
                </svg>
              </div>
              <span class="text-center w-1/5 font-semibold text-sm">${{formatPrice(item.price)}}</span>
              <span class="text-center w-1/5 font-semibold text-sm">${{totalPrice(item.price, item.qty)}}</span>
            </div>

          </div>
          <!--footer-->
          <div className="flex items-center justify-between p-6 border-t border-solid border-slate-200 rounded-b">
            <button
                className="text-red-500 border rounded border-red-500 hover:bg-red-500 hover:text-white background-transparent font-bold uppercase px-6 py-2 text-sm outline-none focus:outline-none mr-1 mb-1 ease-linear transition-all duration-150"
                type="button" v-on:click="toggleModal()">
              Kapat
            </button>
            <button
                className="text-green-500 bg-transparent border border-solid border-green-500 hover:bg-green-500 hover:text-white active:bg-red-600 font-bold uppercase text-sm px-6 py-3 rounded outline-none focus:outline-none mr-1 mb-1 ease-linear transition-all duration-150"
                type="button" v-on:click="toggleModal()">
              Alışverişi Tamamla
            </button>
          </div>
        </div>
      </div>
    </div>
    <div v-if="modal" className="opacity-25 fixed inset-0 z-40 bg-black"></div>
  </div>
</template>