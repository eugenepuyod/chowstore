<script setup>
import { RouterLink } from 'vue-router'
import { computed, ref } from 'vue'
import { onMounted } from 'vue'

onMounted(() => {
  window.scrollTo({ top: 0, behavior: 'smooth' })
})

const cartItems = ref([
  { id: 3, name: 'Adorable Chow Puppy', category: 'Dogs', price: 1500.00, image: '/images/chow_puppy.png', quantity: 1 },
  { id: 1, name: 'Chow Premium Kibble', category: 'Food', price: 45.99, image: '/images/premium_kibble.png', quantity: 2 },
])

const updateQuantity = (id, delta) => {
  const item = cartItems.value.find(i => i.id === id)
  if (item) {
    item.quantity += delta
    if (item.quantity < 1) item.quantity = 1
  }
}

const subtotal = computed(() => {
  return cartItems.value.reduce((sum, item) => sum + (item.price * item.quantity), 0)
})
</script>

<template>
  <div class="bg-slate-50 min-h-screen py-16">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 w-full">
      <h1 class="text-4xl font-extrabold text-slate-900 mb-10 tracking-tight">Your Cart</h1>
      
      <div v-if="cartItems.length > 0" class="grid grid-cols-1 lg:grid-cols-12 gap-12">
        <div class="lg:col-span-8 space-y-6">
          
          <div v-for="item in cartItems" :key="item.id" class="bg-white border text-left border-slate-200 rounded-2xl p-6 flex flex-col sm:flex-row items-center gap-6 shadow-sm hover:shadow-md transition-shadow">
            <div class="w-32 h-32 flex-shrink-0 bg-slate-50 rounded-xl border border-slate-100 p-2">
              <img :src="item.image" :alt="item.name" class="w-full h-full object-cover rounded-lg">
            </div>
            
            <div class="flex-grow flex flex-col justify-center">
              <div class="text-sm font-semibold text-brand-600 mb-1 tracking-wider uppercase">{{ item.category }}</div>
              <h3 class="text-xl font-bold text-slate-800 mb-2">{{ item.name }}</h3>
              <div class="text-2xl font-black text-slate-900 mb-4">₱{{ item.price.toFixed(2) }}</div>
              
              <div class="flex items-center gap-4">
                <div class="flex items-center border border-slate-200 rounded-lg bg-slate-50">
                  <button @click="updateQuantity(item.id, -1)" class="w-10 h-10 flex items-center justify-center text-slate-500 hover:text-brand-600 hover:bg-slate-100 rounded-l-lg transition-colors">-</button>
                  <div class="w-10 h-10 flex items-center justify-center font-bold text-slate-800 bg-white border-x border-slate-200">{{ item.quantity }}</div>
                  <button @click="updateQuantity(item.id, 1)" class="w-10 h-10 flex items-center justify-center text-slate-500 hover:text-brand-600 hover:bg-slate-100 rounded-r-lg transition-colors">+</button>
                </div>
                <button class="text-sm text-rose-500 hover:text-rose-700 font-medium underline transition-colors">Remove</button>
              </div>
            </div>
            <div class="hidden sm:flex self-start sm:self-center ml-auto flex-col items-end">
                <span class="text-sm text-slate-500 font-medium whitespace-nowrap">Total for item</span>
                <span class="text-2xl font-black text-brand-700 whitespace-nowrap">₱{{ (item.price * item.quantity).toFixed(2) }}</span>
            </div>
          </div>
          
        </div>
        
        <div class="lg:col-span-4">
          <div class="bg-white rounded-2xl p-8 border border-slate-200 shadow-sm sticky top-24">
            <h2 class="text-2xl font-bold text-slate-800 mb-6 border-b border-slate-100 pb-4">Order Summary</h2>
            
            <!-- Promo Code Input -->
            <div class="mb-6 border-b border-slate-100 pb-6 hidden">
              <label class="block text-sm font-semibold text-slate-700 mb-2">Promo Code</label>
              <div class="flex gap-2">
                <input type="text" class="flex-grow border border-slate-300 rounded-lg px-4 py-2.5 focus:ring-2 focus:ring-brand-500 focus:outline-none uppercase text-sm font-medium" placeholder="e.g. CHOW20">
                <button class="bg-slate-900 text-white px-5 py-2.5 rounded-lg font-bold hover:bg-brand-600 transition-colors shadow-sm text-sm">Apply</button>
              </div>
            </div>

            <div class="space-y-4 mb-6 text-lg">
              <div class="flex justify-between text-slate-600">
                <span>Subtotal</span>
                <span class="font-medium text-slate-800">₱{{ subtotal.toFixed(2) }}</span>
              </div>
              <div class="flex justify-between text-slate-600">
                <span>Shipping Estimate</span>
                <span class="font-medium text-slate-800">₱150.00</span>
              </div>
              <div class="flex justify-between text-brand-600 hidden">
                <span>Chow Store Discount</span>
                <span class="font-medium">-₱100.00</span>
              </div>
            </div>
            
            <div class="border-t border-slate-100 pt-6 mb-8 mt-2">
              <div class="flex justify-between items-center">
                <span class="text-xl font-bold text-slate-800">Total</span>
                <span class="text-3xl font-black text-slate-900">₱{{ (subtotal + 150).toFixed(2) }}</span>
              </div>
              <p class="text-sm text-slate-500 mt-2 text-right">Taxes calculated at checkout</p>
            </div>
            
            <RouterLink to="/checkout" class="w-full flex items-center justify-center bg-brand-600 hover:bg-brand-500 text-white py-4 rounded-xl font-bold text-lg shadow-[0_4px_14px_0_rgba(14,165,233,0.39)] hover:shadow-[0_6px_20px_rgba(14,165,233,0.23)] hover:-translate-y-1 transition-all duration-200">
              Proceed to Checkout
            </RouterLink>
            <div class="mt-6 text-center">
              <RouterLink to="/shop" class="text-sm text-brand-600 hover:text-brand-800 font-medium">Continue Shopping</RouterLink>
            </div>
          </div>
        </div>
      </div>
      
      <div v-else class="text-center py-20 bg-white rounded-2xl border border-slate-200 shadow-sm">
        <div class="w-32 h-32 mx-auto bg-slate-50 rounded-full flex items-center justify-center mb-6">
          <svg xmlns="http://www.w3.org/2000/svg" width="64" height="64" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round" class="text-slate-300"><circle cx="8" cy="21" r="1"/><circle cx="19" cy="21" r="1"/><path d="M2.05 2.05h2l2.66 12.42a2 2 0 0 0 2 1.58h9.78a2 2 0 0 0 1.95-1.57l1.65-7.43H5.12"/></svg>
        </div>
        <h2 class="text-2xl font-bold text-slate-800 mb-2">Your cart is empty</h2>
        <p class="text-slate-500 mb-8 max-w-md mx-auto">Looks like you haven't added any premium goodies for your Chow Chow yet.</p>
        <RouterLink to="/shop" class="inline-flex items-center justify-center px-8 py-3 border border-transparent text-base font-medium rounded-full text-brand-700 bg-brand-100 hover:bg-brand-200 transition-colors">
          Start Shopping
        </RouterLink>
      </div>
      
    </div>
  </div>
</template>
