<script>
export default {
  props:['products', 'categories', 'addToCart'],
  data() {
    return {
      categoriesProp: this.categories,
      filterCategoriesTitle(id) {
        return (this.categoriesProp.filter((obj) => obj.id === id))[0].title
      },
      filterCategoriesSlug(id) {
        return (this.categoriesProp.filter((obj) => obj.id === id))[0].slug
      }
    }
  },
  methods: {
    formatPrice(value) {
      let val = (value/1).toFixed(2).replace('.', ',')
      return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".")
    }
  }
}
</script>

<template>
<div
    v-for="product in products"
    :key="product.id"
>
<div class="bg-white block rounded-lg shadow-lg relative">
  <img :src="product.image">

  <div class="absolute bg-gradient-to-t flex from-gray-800 h-full hover:opacity-100 items-center justify-center left-0 opacity-0 top-0 w-full">
  <button class="bg-transparent hover:bg-indigo-500 text-white font-semibold hover:text-white py-2 px-4 border border-white-500 hover:border-transparent rounded"
  @click="addToCart(product)"
  >Sepete Ekle</button>
  </div>
</div>
<div class="flex items-center justify-between mt-3">
  <div>
    <a :href="'/product/'+product.slug" class="font-medium">{{ product.title }}</a>
    <a class="flex items-center" :href="'/products/'+filterCategoriesSlug(product.categoryId)">
      <span class="text-xs font-medium text-gray-600">Category:</span>
      <span class="text-xs font-medium ml-1 text-indigo-500">{{ filterCategoriesTitle(product.categoryId) }}</span>
    </a>
  </div>
  <span class="flex items-center h-8 bg-indigo-200 text-indigo-600 text-sm px-2 rounded">${{ formatPrice(product.price) }}</span>
</div>
</div>
</template>