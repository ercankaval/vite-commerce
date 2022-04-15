<script>
export default {
  name: 'basketModule',
  data() {
    return {
      cartItems: [],
      showMessage: false,
      basketTotalItems: 0,
      selectedProductCount: 0,
      selectedProductTotalPrice: 0,
      checkoutTotalPrice: 0,
    }
  },
  created() {
    this.cartItems = JSON.parse(window.localStorage.getItem('cartItems'))
    this.sumTotal()
  },
  methods: {
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
    },
    selectedItem(item) {
      console.log(item)
    }
  }
}
</script>

<template>
  <div>
      <div class="container mx-auto mt-10">
        <div class="flex shadow-md my-10">
          <div class="w-3/4 bg-white px-10 py-10">
            <div class="flex justify-between border-b pb-8">
              <h1 class="font-semibold text-2xl">Sepetinizdeki Ürünler</h1>
              <h2 class="font-semibold text-2xl">{{basketTotalItems}} Adet</h2>
            </div>

            <div class="flex mt-10 mb-5">
              <h3 class="mr-2">#</h3>
              <h3 class="font-semibold text-gray-600 text-xs uppercase w-2/5">Ürün Detayı</h3>
              <h3 class="font-semibold text-center text-gray-600 text-xs uppercase w-1/5 text-center">Adet</h3>
              <h3 class="font-semibold text-center text-gray-600 text-xs uppercase w-1/5 text-center">Fiyat</h3>
              <h3 class="font-semibold text-center text-gray-600 text-xs uppercase w-1/5 text-center">Toplam</h3>
            </div>

            <div
                class="flex items-center hover:bg-gray-100 px-6 py-5 -mx-8"
                v-for="item in cartItems"
                :key="item.id"
                :if="cartItems"
            >
              <div class="flex w-2/5"> <!-- product -->
                <div class="form-check mr-2 flex" style="align-items: center;">
                  <input @change="selectedItem(item)" type="checkbox" value="" id="flexCheckDefault3">
                </div>
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

            <a href="/" class="flex font-semibold text-indigo-600 text-sm mt-10">

              <svg class="fill-current mr-2 text-indigo-600 w-4" viewBox="0 0 448 512"><path d="M134.059 296H436c6.627 0 12-5.373 12-12v-56c0-6.627-5.373-12-12-12H134.059v-46.059c0-21.382-25.851-32.09-40.971-16.971L7.029 239.029c-9.373 9.373-9.373 24.569 0 33.941l86.059 86.059c15.119 15.119 40.971 4.411 40.971-16.971V296z"/></svg>
              Alışverişe Devam Et
            </a>
          </div>

          <div id="summary" class="w-1/4 px-8 py-10">
            <h1 class="font-semibold text-2xl border-b pb-8">Sipariş Özeti</h1>
            <div class="flex justify-between mt-10 mb-5">
              <span class="font-semibold text-sm uppercase">Seçilen {{selectedProductCount}} Adet</span>
              <span class="font-semibold text-sm">${{formatPrice(selectedProductTotalPrice)}}</span>
            </div>
            <div>
              <label class="font-medium inline-block mb-3 text-sm uppercase">Kargo</label>
              <select class="block p-2 text-gray-600 w-full text-sm border">
                <option>UPS Cargo - $10.00</option>
                <option>US Post - $12.00</option>
              </select>
            </div>
            <div class="border-t mt-8">
              <div class="flex font-semibold justify-between py-6 text-sm uppercase">
                <span>Genel Toplam</span>
                <span>${{formatPrice(checkoutTotalPrice)}}</span>
              </div>
              <button class="bg-indigo-500 font-semibold hover:bg-indigo-600 py-3 text-sm text-white uppercase w-full">Ödeme Yap</button>
            </div>
          </div>

        </div>
      </div>
    </div>
</template>

<style></style>
