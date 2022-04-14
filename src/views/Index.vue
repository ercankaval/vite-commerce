<script>
import products from '@/mixins/products.json'
import SingleProduct from "./shared/SingleProduct.vue";
import categories from '@/mixins/categories.json'

export default {
  components: {SingleProduct},
  data() {
    return {
      categoryId: 0,
      selectedCategory: 'Tüm Ürünler',
      products: products.data,
      categories: categories.data,
      cartItems: []
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
    },
    removeItem(index) {
      this.cartItems.splice(this.cartItems.indexOf(index), 1)
      window.localStorage.setItem('cartItems', JSON.stringify(this.cartItems))
    }
  }
}

</script>

<template>
  <div class="sm:container sm:mx-auto">
    <!-- Component Start -->
    <table v-if="cartItems">
      <tr>
        <th>ID</th>
        <th>Title</th>
        <th>Image</th>
        <th>Quantity</th>
        <th>*</th>
      </tr>
      <tr v-for="item in cartItems" :key="item.id">
        <td>{{item.id}}</td>
        <td>{{item.title}}</td>
        <td><img :src="item.image" style="width: 100px"></td>
        <td>{{item.qty}}</td>
        <td><button @click="removeItem(item)">Remove</button></td>
      </tr>
    </table>
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
    <div class="fixed right-1/2 bottom-3 py-3 px-5 rounded-lg border border-green-500 bg-green-400 text-white">
      Secondary Alert
    </div>
  </div>
</template>

<style></style>
