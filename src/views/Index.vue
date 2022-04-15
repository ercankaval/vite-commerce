<script>
import products from '@/mixins/products.json'
import SingleProduct from "./shared/SingleProduct.vue";
import categories from '@/mixins/categories.json'

export default {
  name: 'singleProduct',
  components: {SingleProduct},
  data() {
    return {
      categoryId: 0,
      selectedCategory: 'Tüm Ürünler',
      products: products.data,
      categories: categories.data,
      cartItems: [],
      alertMessage: false,
      alertMessageText: ''
    }
  },
  created() {
    if (window.localStorage.getItem('cartItems'))
      this.cartItems = JSON.parse(window.localStorage.getItem('cartItems'))
  },
  methods: {
    changeCategory(categoryId, categoryTitle) {
      this.categoryId = categoryId
      this.selectedCategory = categoryTitle

      if (categoryId > 0)
        this.products = (products.data.filter((obj) => obj.categoryId === categoryId))
      else
        this.products = products.data
    },
    addToCart(itemToAdd) {
      const itemInCart = this.cartItems.filter(item => item.id===itemToAdd.id)
      const isItemInCart = itemInCart.length > 0

      if (isItemInCart === false) {
        itemToAdd.qty = 1
        this.cartItems.push(itemToAdd);
      } else {
        itemInCart[0].qty ++;
      }

      window.localStorage.setItem('cartItems', JSON.stringify(this.cartItems))
      this.showAlertMessage('Seçtiğiniz ürün sepetinize eklenmiştir.')
    },
    removeItem(index) {
      this.cartItems.splice(this.cartItems.indexOf(index), 1)
      window.localStorage.setItem('cartItems', JSON.stringify(this.cartItems))
      this.showAlertMessage('Seçtiğiniz ürün sepetinizden çıkartılmıştır.')
    },
    showAlertMessage(text) {
      this.alertMessage = true
      this.alertMessageText = text
      setTimeout(() => this.hideAlertMessage(), 1000)
    },
    hideAlertMessage() {
      this.alertMessage = false
    }
  }
}

</script>

<template>
  <div class="sm:container sm:mx-auto">
    <!-- Component Start -->

    <h1 class="text-3xl">
      The North Face
      <span v-if="categoryId > 0">{{ selectedCategory }} Ürünleri</span>
    </h1>
    <div class="flex flex-col sm:flex-row sm:items-end sm:justify-between mt-6 z-10">
      <span class="text-sm font-semibold">{{ products.length }} Products</span>
      <button class="relative text-sm focus:outline-none group mt-4 sm:mt-0">
        <div class="flex items-center justify-between w-40 h-10 px-3 border-2 border-gray-300 rounded hover:bg-gray-300">
        <span class="font-medium">
          {{ selectedCategory }}
        </span>
          <svg class="w-4 h-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
            <path fill-rule="evenodd" d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z" clip-rule="evenodd" />
          </svg>
        </div>
        <div class="absolute flex-col items-start hidden w-full pb-1 bg-white shadow-lg rounded group-focus:flex">
          <a
              class="w-full px-4 py-2 text-left hover:bg-gray-200"
              @click="changeCategory(0, 'Tüm Ürünler')"
          >Tüm Ürünler</a>
          <a
              class="w-full px-4 py-2 text-left hover:bg-gray-200"
              v-for="category in categories"
              :key="category.id"
              @click="changeCategory(category.id, category.title)"
          >{{category.title}}</a>
        </div>
      </button>
    </div>
    <div class="grid 2xl:grid-cols-5 xl:grid-cols-4 lg:grid-cols-3 sm:grid-cols-2 grid-cols-1 gap-x-6 gap-y-12 w-full mt-6">
     <!-- Products -->
      <SingleProduct :products="products" :categories="categories" :addToCart="addToCart" />
    </div>
    <!-- Component End  -->

      <div v-if="alertMessage" class="fixed right-3 bottom-3">
        <div class="bg-white border-t-4 rounded-b px-4 py-3 shadow-md" role="alert">
          <div class="flex">
            <div class="py-1"><svg class="fill-current h-6 w-6 mr-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><path d="M2.93 17.07A10 10 0 1 1 17.07 2.93 10 10 0 0 1 2.93 17.07zm12.73-1.41A8 8 0 1 0 4.34 4.34a8 8 0 0 0 11.32 11.32zM9 11V9h2v6H9v-4zm0-6h2v2H9V5z"/></svg></div>
            <div>
              <p class="font-bold relative">
                <span>Bilgi</span>
                <span @click="hideAlertMessage()" class="absolute top-0 bottom-0 right-0">
                  <svg class="fill-current h-6 w-6 text-white-500" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><title>Close</title><path d="M14.348 14.849a1.2 1.2 0 0 1-1.697 0L10 11.819l-2.651 3.029a1.2 1.2 0 1 1-1.697-1.697l2.758-3.15-2.759-3.152a1.2 1.2 0 1 1 1.697-1.697L10 8.183l2.651-3.031a1.2 1.2 0 1 1 1.697 1.697l-2.758 3.152 2.758 3.15a1.2 1.2 0 0 1 0 1.698z"/></svg>
                </span>
              </p>
              <p class="text-sm">{{alertMessageText}}</p>
            </div>
          </div>
        </div>
    </div>
  </div>
</template>

<style></style>
