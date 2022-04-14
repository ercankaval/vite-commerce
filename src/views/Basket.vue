<script>
export default {
  data() {
    return {
      cartItems: []
    }
  },
  created() {
    this.getBasketItems()
  },
  methods: {
    getBasketItems() {
      this.cartItems = JSON.parse(window.localStorage.getItem('cartItems'))
    }
  }
}
</script>

<template>
  <div>
    <h1>Basket</h1>
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
  </div>
</template>

<style></style>
