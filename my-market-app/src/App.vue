<template>
 <div :class="isDark ? 'dark' : ''" class="app">

    <!-- BACKGROUND -->
    <div class="bg-gradient"></div>

    <!-- NAVBAR -->
    <header class="nav-glass">
      
          <input 
                 v-if="page==='home' || page==='shop'"
                 v-model="search" 
                 placeholder="Search" 
                 class="search"/>

       <div class="nav-actions">
        <button class="btn"  @click="page='home'">Home </button>
        <button class="btn" @click="page='shop'">Shop</button>
        <button class="btn" @click="page='login'">Login</button>
        <button class="btn" @click="page='signup'">Sign Up</button>
        <button @click="isDark = !isDark"> {{ isDark ? "☀️" : "🌙" }} </button>
        <button class="btn btn-primary" @click="showCart = true">🛒 {{ cart.length }}</button>
      </div>
    </header>

 <!-- 🔥 Home-->
<section v-if="page==='home'" class="hero-pro">
    
    <div class="hero-overlay">
      <h1>Snappick Market</h1>
      <p>Find the best deals on trending products</p>
      <button class="btn btn-primary small-btn" @click="page='shop'">Shop Now</button>
    </div>
</section>
    <div v-if="page=='home'">
    <div class="home-section">
       <h2 class="section-title">✨ Why Choose Us</h2>
    <div class="features">
    <div class="feature-card">🚚 Fast Delivery</div>
    <div class="feature-card">💳 Secure Payments</div>
    <div class="feature-card">⭐ Top Quality</div>
    <div class="feature-card">🔄 Easy Returns</div>
    </div>
    </div>

<div class="promo-banner">
  <h2>🔥 Mega Sale</h2>
  <p>Up to 50% OFF on selected items</p>
  <button class="btn btn-primary" @click="page='shop'">
    Shop Now
  </button>
</div>

<div class="testimonials">
  <h2>💬 What Customers Say</h2>

  <div class="testimonial-grid">
    <div class="testimonial-card">"Amazing quality!" ⭐⭐⭐⭐⭐</div>
    <div class="testimonial-card">"Fast delivery!" ⭐⭐⭐⭐⭐</div>
    <div class="testimonial-card">"Best prices!" ⭐⭐⭐⭐⭐</div>
  </div>
</div>

<div class="brand-section">
  <h2>Shop Anytime, Anywhere</h2>
  <p>Available on mobile & desktop</p>

<div class="brand-list">
  <div class="brand-section-item">🚚 Free Shipping</div>
  <div class="brand-section-item">🔒 Secure Payment</div>
  <div class="brand-section-item">📞 24/7 Support</div>
</div>
</div>

<div class="subscribe">
  <h2>Subscribe</h2>
  <div class="subscribe-box">
  <input placeholder="Enter your email"/>
  <button >Subscribe</button>
</div>
</div>

<footer class="footer">
  <p>© 2026 Snappick Market</p>
  <p>Contact | About | Privacy</p>
</footer>
</div>



<!-- 🔥 SECTION TITLE -->
<div v-if="page==='shop'" class="section-title">
  <h2> 🛍️ All Products</h2>
</div>

    <!-- CARDS -->
    <div v-if="page==='shop'" class="grid">
      <div v-for="p in filtered" :key="p.id" class="card">

        <img :src="p.thumbnail" @click="view(p)" style="cursor:pointer"/>

        <div class="card-body">
          <h3>{{ p.title }}</h3>
          <p>Rs.{{ p.price }}</p>

          <div class="actions">
            <button @click="view(p)">View</button>
            <button class="primary" @click="add(p)">Add to Cart</button>
          </div>
        </div>

      </div>
    </div>

  <!-- 🔥 PRODUCT DETAILS MODAL (UPGRADED) -->
<div v-if="selected" class="modal">
  <div class="modal-card">

  <!-- ❌ CLOSE BUTTON -->
    <button class="close-btn" @click="selected=null">✖</button>

    <!-- LEFT IMAGE -->
    <div class="modal-left">
      <img :src="selected.thumbnail" />
    </div>

    <!-- RIGHT DETAILS -->
    <div class="modal-right">
      <h2>{{ selected.title }}</h2>

      <p class="desc">{{ selected.description }}</p>

      <div class="price">Rs. {{ selected.price }}</div>

      <div class="rating">⭐⭐⭐⭐⭐ (4.8)</div>

      <!-- buttons -->
      <div class="modal-actions">
        <button class="btn btn-primary" @click="add(selected)">
          🛒 Add to Cart
        </button>

        <button class="btn" @click="selected=null">
          Close
        </button>
      </div>

    </div>

  </div>
</div>

    
    <!-- LOGIN -->
<div v-if="page==='login'" class="auth">
  <div class="auth-wrapper">

    <!-- LEFT SIDE -->
    <div class="auth-left">
      <h1>Welcome Back 👋</h1>
      <p>Login to continue your learning journey</p>
    </div>

    <!-- RIGHT SIDE -->
    <div class="auth-card">
      <h2>Login</h2>

      <input v-model="login.email" placeholder="Email"/>
      <input v-model="login.password" type="password" placeholder="Password"/>

      <button @click="handleLogin">Login</button>

      <p class="link" @click="page='signup'">
        Don't have an account? Sign up
      </p>
    </div>

  </div>
</div>

    <!-- SIGNUP -->
<div v-if="page==='signup'" class="auth">
  <div class="auth-wrapper">

    <div class="auth-left">
      <h1>Create Account 🚀</h1>
      <p>Join us and start learning today</p>
    </div>

    <div class="auth-card">
      <h2>Sign Up</h2>

      <input v-model="signup.name" placeholder="Name"/>
      <input v-model="signup.email" placeholder="Email"/>
      <input v-model="signup.password" type="password" placeholder="Password"/>

      <button @click="handleSignup">Create Account</button>

      <p class="link" @click="page='login'">
        Already have an account? Login
      </p>
    </div>

  </div>
</div>

   <!-- CART -->
<div v-if="showCart" class="cart-overlay">

  <div class="cart-panel">

    <h2>🛒 My Cart</h2>

    <!-- ITEMS -->
    <div v-if="cart.length === 0" class="empty">
      Your cart is empty
    </div>

    <div v-for="(c, index) in cart" :key="index" class="cart-item">

  <span>{{ c.title }}</span>
  <span>Rs. {{ c.price }}</span>

  <button class="remove-btn" @click="removeFromCart(index)">
    ❌
  </button>

</div>
    <!-- TOTAL -->
    <div class="cart-total">
      Total: <b>Rs. {{ total }}</b>
    </div>

    <!-- PAYMENT -->
    <h4>Payment Methods</h4>

    <button class="pay-btn">💳 Credit Card</button>
    <button class="pay-btn">📱 Mobile Payment</button>
    <button class="pay-btn">🏦 Bank Transfer</button>

    <!-- ACTIONS -->
    <button class="checkout">Checkout</button>
    <button class="back-btn" @click="showCart=false">⬅ Go Back</button>
   </div>
  </div>

</div>
</template>

<script setup lang="ts">

import { ref, computed, onMounted } from "vue"

const search = ref("")
const selected = ref<Product | null>(null)
const cart = ref<Product[]>([])
const showCart = ref(false)
const isDark = ref(false)
const page = ref("home")
const removeFromCart = (index) => {
  cart.value.splice(index, 1)
}
type Product = {
  id: number
  title: string
  price: number
  description: string
  thumbnail: string
}
const products = ref<Product[]>([])

const login = ref({ email:"", password:"" })
const signup = ref({ name:"", email:"", password:"" })

/* FETCH + YOUR NAMES + PRICES */
onMounted(async () => {
  const res = await fetch("https://dummyjson.com/products?limit=30")
  const data = await res.json()

  products.value = data.products
})

/* FILTER */
const filtered = computed(() =>
  products.value.filter(p =>
    p.title.toLowerCase().includes(search.value.toLowerCase())
  )
)

/* FUNCTIONS */
const view = (p) => selected.value = p
const add = (p) => cart.value.push(p)

const total = computed(() =>
  cart.value.reduce((t, i) => t + i.price, 0)
)

/* AUTH */
const handleLogin = () => {
  if(login.value.email && login.value.password){
    alert("Login successful!")
    page.value = "home"
  } else {
    alert("Fill all fields!")
  }
}

const handleSignup = () => {
  if(signup.value.name && signup.value.email && signup.value.password){
    alert("Account created!")
    page.value = "home"
  } else {
    alert("Fill all fields!")
  }
}
</script>

<style>
body {
  margin: 0;
  font-family: 'Poppins', sans-serif;
}

.bg-gradient {
  position: fixed;
  width: 100%;
  height: 100%;
  background: linear-gradient(135deg, #4d086841,  #4d086841,  #4d086841);
  z-index: -1;
}

.nav-glass {
  display: flex;
  justify-content: space-between;
  padding: 15px 30px;
  backdrop-filter: blur(10px);
  background: rgba(255,255,255,0.2);
  color: white;
}

.logo {
  font-size: 22px;
  font-weight: bold;
  cursor:pointer;
}
html, body {
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
  overflow-x: hidden;
}
.app {
  width: 100%;
  min-height: 100vh;
}

.search {
  padding: 8px 15px;
  border-radius: 20px;
  border: none;
  font-size:20px;
}

.nav-actions {
  padding: 10px 25px;  
  display: flex;
  gap: 10px;
  border-radius:9999px;
}

.hero {
  text-align: center;
  color: white;
  padding: 100px 20px;
}

.hero h1 {
  font-size: 75px;
}

.hero p{
  font-size: 25px;
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px,1fr));
  gap: 20px;
  padding: 40px;
}

.card {
  backdrop-filter: blur(12px);
  background: rgba(255,255,255,0.15);
  border-radius: 20px;
  overflow: hidden;
  color: white;
  transition: 0.3s;
}

.card:hover {
  transform: translateY(-10px) scale(1.03);
}

.card img {
  width: 100%;
  height: 180px;
  object-fit: cover;
}

.card-body {
  padding: 15px;
}

.actions {
  display: flex;
  gap: 10px;
  margin-top: 10px;
}

.primary {
  background: #ff7a18;
  color: white;
  border:none;
  padding:5px 10px;
}

button {
  border-radius: 5000px;
}

/* 🌙 DARK MODE FIX */
.dark {
  background: #0f172a;
}

/* background */


/* navbar */
.dark .nav-glass {
  background: rgba(0, 0, 0, 0.4);
  color: white;
}

/* cards */
.dark .card {
  background: rgba(255, 255, 255, 0.05);
  color: white;
}



/* cart */
.dark .cart {
  background: #1e293b;
  color: white;
}

/* auth boxes */
.dark .auth-card {
  background: #1e293b;
  color: white;
}

/* input fields */
.dark input {
  background: #0f172a;
  color: white;
  border: 1px solid #334155;
}

/* 🌟 AUTH LAYOUT */
.auth {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

/* container split */
.auth-wrapper {
  display: flex;
  width: 800px;
  height: 450px;
  border-radius: 20px;
  overflow: hidden;
  box-shadow: 0 10px 40px rgba(0,0,0,0.2);
  backdrop-filter: blur(10px);
}

/* LEFT SIDE */
.auth-left {
  flex: 1;
  background: linear-gradient(135deg, #667eea, #6ccef5);
  color: white;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 40px;
}

.auth-left h1 {
  font-size: 32px;
}

.auth-left p {
  opacity: 0.9;
}

/* RIGHT SIDE FORM */
.auth-card {
  flex: 1;
  background: white;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 40px;
  gap: 12px;
}

.auth-card h2 {
  margin-bottom: 10px;
}

/* INPUTS */
.auth-card input {
  padding: 12px;
  border-radius: 10px;
  border: 1px solid #ddd;
  outline: none;
}

/* BUTTON */
.auth-card button {
  padding: 12px;
  border-radius: 10px;
  border: none;
  background: #ff7a18;
  color: white;
  cursor: pointer;
}

/* LINK */
.link {
  color: #667eea;
  cursor: pointer;
  font-size: 14px;
}

/* DARK MODE SUPPORT */
.dark .auth-card {
  background: #1e293b;
  color: white;
}

.dark .auth-card input {
  background: #0f172a;
  color: white;
  border: 1px solid #334155;
}

/* CART OVERLAY */
.cart-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.6);
  display: flex;
  justify-content: flex-end;
}

/* CART PANEL */
.cart-panel {
  width: 450px;
  height: 100%;
  background: white;
  padding: 20px;
  overflow-y: auto;
  animation: slideIn 0.3s ease;
}

/* animation */
@keyframes slideIn {
  from { transform: translateX(100%); }
  to { transform: translateX(0); }
}

/* items */
.cart-item {
  display: flex;
  justify-content: space-between;
  padding: 8px 0;
  border-bottom: 1px solid #eee;
}

/* total */
.cart-total {
  margin-top: 15px;
  font-size: 18px;
}

/* payment buttons */
.pay-btn {
  width: 100%;
  padding: 10px;
  margin-top: 8px;
  border: none;
  border-radius: 10px;
  background: #f1f1f1;
  cursor: pointer;
}

.pay-btn:hover {
  background: #ddd;
}

/* checkout */
.checkout {
  width: 100%;
  margin-top: 15px;
  padding: 12px;
  background: green;
  color: white;
  border: none;
  border-radius: 10px;
}

/* back button */
.back-btn {
  width: 100%;
  margin-top: 10px;
  padding: 12px;
  background: #ff7a18;
  color: white;
  border: none;
  border-radius: 10px;
}

/* empty cart */
.empty {
  text-align: center;
  color: gray;
  padding: 20px;
}

/* DARK MODE */
.dark .cart-panel {
  background: #1e293b;
  color: white;
}

.dark .pay-btn {
  background: #0f172a;
  color: white;
}

.dark .pay-btn:hover {
  background: #334155;
}

.home-btn {
  padding: 8px 20px;
  border-radius: 9999px; /* makes oval */
  border: none;
  background: rgba(255,255,255,0.3);
  color: white;
  font-weight: bold;
  cursor: pointer;
  backdrop-filter: blur(8px);
  transition: 0.3s;
}

/* hover effect */
.home-btn:hover {
  background: white;
  color: #667eea;
}

.dark .home-btn {
  background: rgba(0,0,0,0.4);
  color: white;
}

.dark .home-btn:hover {
  background: white;
  color: black;
}

.btn {
  padding: 8px 20px;
  border-radius: 9999px; /* oval shape */
  border: none;
  background: rgba(255,255,255,0.25);
  color: white;
  font-weight: 500;
  cursor: pointer;
  backdrop-filter: blur(8px);
  transition: 0.3s;
   font-size: 16px;
}
.small-btn {
  padding: 15px 2px;
  font-size:20px;
}

/* hover */
.btn:hover {
  background: white;
  color: #667eea;
}

/* primary button (important actions) */
.btn-primary {
  background: linear-gradient(135deg, #ff7a18, #ffb347);
  color: white;
  border-radius:10px%;
}

.btn-primary:hover {
  opacity: 0.9;
}

.dark .btn {
  background: rgba(0,0,0,0.4);
  color: white;
}

.dark .btn:hover {
  background: white;
  color: black;
}

/* 🔥 HERO ADVANCED */
.hero-advanced {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 80px 40px;
  color: white;
}

.hero-left {
  max-width: 500px;
}

.hero-left h1 {
  font-size: 50px;
}

.hero-left p {
  margin: 15px 0;
  font-size: 18px;
}

.hero-buttons {
  display: flex;
  gap: 10px;
}

/* images */
.hero-right {
  display: flex;
  gap: 15px;
}

.hero-right img {
  width: 120px;
  height: 120px;
  border-radius: 20px;
  object-fit: cover;
  transition: 0.3s;
}

.hero-right img:hover {
  transform: scale(1.1);
}

/* 🔥 CATEGORIES */
.categories {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px,1fr));
  gap: 20px;
  padding: 20px 40px;
}

.cat-card {
  background: rgba(255,255,255,0.2);
  backdrop-filter: blur(10px);
  border-radius: 20px;
  text-align: center;
  padding: 15px;
  cursor: pointer;
  transition: 0.3s;
}

.cat-card:hover {
  transform: translateY(-5px);
}

.cat-card img {
  width: 100%;
  height: 100px;
  object-fit: cover;
  border-radius: 15px;
}

.cat-card h3 {
  margin-top: 10px;
  color: white;
}

/* 🔥 PROMO */
.promo-banner {
  margin: 30px 40px;
  padding: 30px;
  border-radius: 20px;
  background: linear-gradient(135deg, #ff7a18, #ffb347);
  color: white;
  text-align: center;
}

/* 🔥 HERO PROFESSIONAL */
.hero-pro {
  position: relative;
  height: 600px;
  margin: 20px auto;
  border-radius: 20px;
  overflow: hidden;
  max-width:100%;
}

/* background image */
.hero-bg {
  width: 100%;
  height: 100%;
  object-fit: cover;
  filter:brightness(0.7);
}

/* dark overlay */
.hero-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(82, 75, 75, 0.89);
  

  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 40px;
  color: white;
}

.hero-overlay h1 {
  font-size: 75px;
}

.hero-overlay p {
  margin: 10px 0;
  font-size: 18px;
}

/* 🔥 CATEGORY BAR */
.category-bar {
  display: flex;
  gap: 20px;
  padding: 20px 40px;
  overflow-x: auto;
}

.category-item {
  min-width: 120px;
  text-align: center;
  cursor: pointer;
}

.category-item img {
  width: 100%;
  height: 80px;
  object-fit: cover;
  border-radius: 15px;
}

.category-item span {
  display: block;
  margin-top: 5px;
  color: white;
}

/* 🔥 TITLE */
.section-title {
  padding: 10px 40px;
  color: white;
}

.section-title h2 {
  font-size: 22px;
}



.hero-overlay {
  background: linear-gradient(
    to right,
    rgba(179, 81, 81, 0.7),
    rgba(136, 138, 253, 0.2)
  );
}

/* 🔥 GLOBAL SECTION */
.home-section {
  padding: 60px 40px;
  max-width: 1100px;
  margin: auto;
  text-align: center;
}

/* titles */
.section-title {
  font-size: 28px;
  margin-bottom: 30px;
  color: white;
  font-weight: 600;
}

/* 🔥 FEATURES */
.features {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px,1fr));
  gap: 20px;
}

.feature-card {
  background: rgba(255,255,255,0.15);
  backdrop-filter: blur(10px);
  padding: 25px;
  border-radius: 15px;
  color: white;
  font-size: 16px;
  transition: 0.3s;
}

.feature-card:hover {
  transform: translateY(-5px);
  background: rgba(255,255,255,0.25);
}

/* 🔥 PROMO BANNER */
.promo-banner {
  margin: 40px auto;
  padding: 40px;
  border-radius: 20px;
  max-width: 1000px;
  text-align: center;
  background: linear-gradient(135deg, #ff7a18, #ffb347);
  color: white;
}

.promo-banner h2 {
  font-size: 26px;
}

.promo-banner p {
  margin: 10px 0;
}

/* 🔥 TESTIMONIALS */
.testimonials {
  padding: 60px 40px;
}

.testimonials h2 {
  color: white;
  margin-bottom: 30px;
  font-size: 26px;
}

.testimonial-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
}

.testimonial-card {
  padding: 20px;
  border-radius: 16px;
  background: rgba(255,255,255,0.15);
  backdrop-filter: blur(10px);
  color: white;
  transition: 0.3s;
}

.testimonial-card:hover {
  transform: translateY(-5px);
  background: rgba(255,255,255,0.25);
}

/* 🔥 BRAND / INFO */
.brand-section {
  text-align: center;
  padding: 60px 20px;
  color: white;
}

.brand-section h2 {
  font-size: 28px;
  margin-bottom: 10px;
}

.brand-section p {
  opacity: 0.8;
  margin-bottom: 30px;
}

.brand-list {
  display: flex;
  justify-content: center;
  gap: 20px;
  flex-wrap: wrap;
}

.brand-section-item {
  padding: 12px 20px;
  border-radius: 999px;
  background: rgba(255,255,255,0.15);
  backdrop-filter: blur(8px);
  transition: 0.3s;
}

.brand-section-item:hover {
  background: white;
  color: #667eea;
}

/* 🔥 SERVICES */
.services {
  display: flex;
  justify-content: center;
  gap: 30px;
  margin-top: 20px;
  flex-wrap: wrap;
}

.services div {
  background: rgba(255,255,255,0.15);
  padding: 15px 25px;
  border-radius: 30px;
  color: white;
}

/* 🔥 subscribe */
.subscribe {
  text-align: center;
  padding: 60px 20px;
  color: white;
}

.subscribe-box {
  margin-top: 20px;
  display: flex;
  justify-content: center;
  gap: 10px;
}

.subscribe-box input {
  padding: 12px 20px;
  border-radius: 999px;
  border: none;
  width: 250px;
  outline: none;
}

.subscribe-box button {
  padding: 12px 25px;
  border-radius: 999px;
  border: none;
  background: linear-gradient(135deg, #ff7a18, #ffb347);
  color: white;
  cursor: pointer;
  transition: 0.3s;
}

.subscribe-box button:hover {
  transform: scale(1.05);
}

/* 🔥 FOOTER */
.footer {
  margin-top: 60px;
  padding: 30px;
  text-align: center;
  color: white;
  background: rgba(0,0,0,0.3);
}

section, .testimonials, .features, .subscribe {
  margin-top: 40px;
} 

/* 🔥 MODAL BACKGROUND */
.modal {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.7);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 999;
}

/* 🔥 MODAL CARD */
.modal-card {
  display: flex;
  gap: 30px;
  width: 800px;
  max-width: 90%;
  padding: 25px;
  border-radius: 20px;
  background: white;
  animation: fadeIn 0.3s ease;
  position:relative;
}

/* animation */
@keyframes fadeIn {
  from { transform: scale(0.9); opacity: 0 }
  to { transform: scale(1); opacity: 1 }
}

/* LEFT IMAGE */
.modal-left img {
  width: 300px;
  height: 300px;
  object-fit: cover;
  border-radius: 15px;
}

/* RIGHT SIDE */
.modal-right {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

/* title */
.modal-right h2 {
  margin-bottom: 10px;
}

/* description */
.desc {
  font-size: 14px;
  opacity: 0.8;
  margin-bottom: 15px;
}

/* price */
.price {
  font-size: 22px;
  font-weight: bold;
  margin-bottom: 10px;
  color: #ff7a18;
}

/* rating */
.rating {
  margin-bottom: 20px;
  font-size: 14px;
}

/* buttons */
.modal-actions {
  display: flex;
  gap: 10px;
}

/* DARK MODE */
.dark .modal-card {
  background: #1e293b;
  color: white;
}

/* ❌ CLOSE BUTTON */
.close-btn {
  z-index:10;
  position: absolute;
  top: 15px;
  right: 20px;
  border: none;
  border-radius: 50%;
  background: rgba(0,0,0,0.6);
  color: white;
  font-size: 18px;
  width: 35px;
  height: 35px;
  border-radius: 50%;
  cursor: pointer;
  transition: 0.3s;
}

.remove-btn {
  background: red;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 10px;
  cursor: pointer;
}

</style>