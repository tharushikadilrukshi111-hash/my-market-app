<template>
  <div id="app">
    <div class="header">
      <h1>Mobile Market</h1>
      <div class="cart-box">🛒 Cart: {{ cartCount }}</div>
    </div>

    <div class="product-container">
      <div v-for="item in products" :key="item.id" class="product-card">
        <div class="product-img">📦</div>
        <h3>{{ item.name }}</h3>
        <p>Rs. {{ item.price }}</p>
        <button @click="addToCart">Add to Cart</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      cartCount: 0,
      products: [
        { id: 1, name: "Smart Watch", price: 12000 },
        { id: 2, name: "Bluetooth Speaker", price: 4500 },
        { id: 3, name: "Gaming Mouse", price: 3200 }
      ]
    }
  },
  methods: {
    addToCart() {
      this.cartCount++;
    }
  }
}
</script>

<style>
body { background-color: #f4f4f4; margin: 0; font-family: sans-serif; }
.header { background: #333; color: white; padding: 15px; display: flex; justify-content: space-between; align-items: center; }
.product-container { display: flex; flex-wrap: wrap; justify-content: center; padding: 20px; }
.product-card { background: white; border-radius: 10px; padding: 15px; margin: 10px; width: 160px; box-shadow: 0 4px 6px rgba(0,0,0,0.1); text-align: center; }
.product-img { font-size: 50px; background: #eee; border-radius: 8px; padding: 10px; margin-bottom: 10px; }
button { background: #ff9900; border: none; padding: 10px; width: 100%; border-radius: 5px; color: white; cursor: pointer; font-weight: bold; }
button:hover { background: #e68a00; }
</style>
