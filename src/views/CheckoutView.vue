<script setup>
import { ref, computed } from 'vue'

const isProcessing = ref(false)
const orderPlaced = ref(false)
const selectedPayment = ref('Visa')

// Promo Logic
const promoInput = ref('')
const promoApplied = ref(false)
const promoError = ref('')

const subtotal = 1591.98
const shipping = 150.00

const discountAmount = computed(() => {
  return promoApplied.value ? (subtotal * 0.20) : 0
})

const total = computed(() => {
  return subtotal + shipping - discountAmount.value
})

const applyPromo = () => {
  promoError.value = ''
  if (promoInput.value.trim().toUpperCase() === 'CHOW20') {
    promoApplied.value = true
  } else {
    promoApplied.value = false
    promoError.value = 'Invalid promo code. Note: try CHOW20'
  }
}

const paymentMethods = [
  // { id: 'Visa', icon: '💳' },
  // { id: 'Mastercard', icon: '💳' },
  // { id: 'PayPal', icon: '🅿️' },
  // { id: 'GCash', icon: '📱' },

  { id: 'Visa', icon: '/images/visa.png' },
  { id: 'Mastercard', icon: '/images/mastercard.png' },
  { id: 'PayPal', icon: '/images/paypal.png' },
  { id: 'GCash', icon: '/images/gcash.png' },
  
]

const processCheckout = () => {
  isProcessing.value = true
  setTimeout(() => {
    isProcessing.value = false
    orderPlaced.value = true
  }, 2000)
}
</script>

<template>
  <div class="bg-white min-h-screen py-16">
    <div class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8 w-full">
      <div v-if="!orderPlaced">
        <h1 class="text-3xl font-extrabold text-slate-900 mb-8 border-b pb-4">Secure Checkout</h1>
        
        <div class="grid grid-cols-1 md:grid-cols-12 gap-12">
          <!-- Checkout Form -->
          <div class="md:col-span-7 space-y-8">
            <section>
              <h2 class="text-xl font-bold text-slate-800 mb-4">Contact Information</h2>
              <div class="space-y-4">
                <div>
                  <label class="block text-sm font-medium text-slate-700 mb-1">Email Address</label>
                  <input type="email" class="w-full border border-slate-300 rounded-lg px-4 py-3 focus:ring-2 focus:ring-brand-500 focus:border-brand-500 transition-colors" placeholder="you@example.com">
                </div>
              </div>
            </section>

            <section>
              <h2 class="text-xl font-bold text-slate-800 mb-4">Shipping Address</h2>
              <div class="grid grid-cols-2 gap-4">
                <div class="col-span-2 sm:col-span-1">
                  <label class="block text-sm font-medium text-slate-700 mb-1">First Name</label>
                  <input type="text" class="w-full border border-slate-300 rounded-lg px-4 py-3 focus:ring-2 focus:ring-brand-500 focus:border-brand-500 transition-colors">
                </div>
                <div class="col-span-2 sm:col-span-1">
                  <label class="block text-sm font-medium text-slate-700 mb-1">Last Name</label>
                  <input type="text" class="w-full border border-slate-300 rounded-lg px-4 py-3 focus:ring-2 focus:ring-brand-500 focus:border-brand-500 transition-colors">
                </div>
                <div class="col-span-2">
                  <label class="block text-sm font-medium text-slate-700 mb-1">Address</label>
                  <input type="text" class="w-full border border-slate-300 rounded-lg px-4 py-3 focus:ring-2 focus:ring-brand-500 focus:border-brand-500 transition-colors" placeholder="123 Chow Avenue">
                </div>
              </div>
            </section>

            <section>
              <h2 class="text-xl font-bold text-slate-800 mb-4">Payment Method</h2>
              <div class="grid grid-cols-2 sm:grid-cols-4 gap-4">
                <div 
                  v-for="method in paymentMethods" 
                  :key="method.id"
                  @click="selectedPayment = method.id"
                  :class="[
                    'border-2 rounded-xl p-2 cursor-pointer text-center transition-all',
                    selectedPayment === method.id 
                      ? 'border-brand-500 bg-brand-50 shadow-md ring-1 ring-brand-500' 
                      : 'border-slate-200 hover:border-brand-300 hover:bg-slate-50'
                  ]"
                >
                  <!-- <div class="text-2xl mb-2">{{ method.icon }}</div>
                  <div class="font-semibold text-sm">{{ method.id }}</div> -->
                <img 
                  :src="method.icon" 
                  :alt="method.id"
                  class="w-20 h-20 object-contain mx-auto"
                />
                </div>
              </div>

              <!-- Card Details Mock -->
              <div v-if="['Visa', 'Mastercard'].includes(selectedPayment)" class="mt-6 p-6 bg-slate-50 rounded-xl border border-slate-200 space-y-4">
                <div>
                  <label class="block text-sm font-medium text-slate-700 mb-1">Card Number</label>
                  <input type="text" class="w-full border border-slate-300 rounded-lg px-4 py-3 focus:ring-2 focus:ring-brand-500 bg-white" placeholder="0000 0000 0000 0000">
                </div>
                <div class="grid grid-cols-2 gap-4">
                  <div>
                    <label class="block text-sm font-medium text-slate-700 mb-1">Expiry</label>
                    <input type="text" class="w-full border border-slate-300 rounded-lg px-4 py-3 focus:ring-2 focus:ring-brand-500 bg-white" placeholder="MM/YY">
                  </div>
                  <div>
                    <label class="block text-sm font-medium text-slate-700 mb-1">CVC</label>
                    <input type="text" class="w-full border border-slate-300 rounded-lg px-4 py-3 focus:ring-2 focus:ring-brand-500 bg-white" placeholder="123">
                  </div>
                </div>
              </div>
              
              <div v-else-if="selectedPayment === 'GCash'" class="mt-6 p-6 bg-blue-50 text-blue-800 rounded-xl border border-blue-200 text-sm text-center">
                You will be redirected to the GCash portal to authorize this transaction securely.
              </div>
              
              <div v-else-if="selectedPayment === 'PayPal'" class="mt-6 p-6 bg-amber-50 text-amber-800 rounded-xl border border-amber-200 text-sm text-center">
                You will be redirected to PayPal to complete your purchase securely.
              </div>

            </section>
            
            <button 
              @click="processCheckout"
              :disabled="isProcessing"
              class="w-full flex items-center justify-center bg-slate-900 hover:bg-brand-600 text-white py-4 rounded-xl font-bold text-lg shadow-lg hover:shadow-xl transition-all"
            >
              <svg v-if="isProcessing" class="animate-spin -ml-1 mr-3 h-5 w-5 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
              </svg>
              {{ isProcessing ? 'Processing...' : `Pay ₱${total.toFixed(2)} via ${selectedPayment}` }}
            </button>
          </div>

          <!-- Order Summary -->
          <div class="md:col-span-5">
            <div class="bg-slate-50 rounded-2xl p-6 border border-slate-200 sticky top-24">
              <h2 class="text-xl font-bold text-slate-800 mb-6 border-b border-slate-200 pb-4">Order Summary</h2>
              
              <!-- Promo Code Input -->
              <div class="mb-6 border-b border-slate-200 pb-6">
                <label class="block text-sm font-semibold text-slate-700 mb-2">Promo Code</label>
                <div class="flex flex-wrap gap-2">
                  <input v-model="promoInput" @keyup.enter="applyPromo" type="text" class="flex-grow border border-slate-300 rounded-lg px-4 py-2.5 focus:ring-2 focus:ring-brand-500 focus:outline-none uppercase text-sm font-medium" placeholder="e.g. CHOW20">
                  <button @click="applyPromo" class="bg-slate-900 text-white px-5 py-2.5 rounded-lg font-bold hover:bg-brand-600 transition-colors shadow-sm text-sm">Apply</button>
                </div>
                <p v-if="promoError" class="text-rose-500 text-xs mt-2 font-medium">{{ promoError }}</p>
                <p v-if="promoApplied" class="text-green-600 text-xs mt-2 font-medium">CHOW20 applied! 20% off your subtotal.</p>
              </div>

              <div class="space-y-4 mb-6">
                <div class="flex items-center gap-4">
                  <div class="w-16 h-16 bg-white rounded-lg border flex items-center justify-center p-2 relative">
                    <img src="/images/chow_puppy.png" alt="Item" class="object-cover rounded">
                    <span class="absolute -top-2 -right-2 bg-slate-500 text-white w-5 h-5 rounded-full flex items-center justify-center text-xs">1</span>
                  </div>
                  <div class="flex-grow">
                     <div class="font-medium text-slate-800">Adorable Chow Puppy</div>
                     <div class="text-sm text-slate-500">Dogs</div>
                  </div>
                  <div class="font-semibold">₱1500.00</div>
                </div>
                
                <div class="flex items-center gap-4">
                  <div class="w-16 h-16 bg-white rounded-lg border flex items-center justify-center p-2 relative">
                    <img src="/images/premium_kibble.png" alt="Item" class="object-cover rounded">
                    <span class="absolute -top-2 -right-2 bg-slate-500 text-white w-5 h-5 rounded-full flex items-center justify-center text-xs">2</span>
                  </div>
                  <div class="flex-grow">
                    <div class="font-medium text-slate-800">Chow Premium Kibble</div>
                    <div class="text-sm text-slate-500">Food</div>
                  </div>
                  <div class="font-semibold">₱91.98</div>
                </div>
              </div>
              
              <div class="border-t border-slate-200 pt-4 space-y-3">
                <div class="flex justify-between text-slate-600">
                  <span>Subtotal</span>
                  <span>₱{{ subtotal.toFixed(2) }}</span>
                </div>
                <div class="flex justify-between text-slate-600">
                  <span>Shipping</span>
                  <span>₱{{ shipping.toFixed(2) }}</span>
                </div>
                <div v-if="promoApplied" class="flex justify-between text-brand-600 font-medium transition-all">
                  <span>Chow Discount (20%)</span>
                  <span>-₱{{ discountAmount.toFixed(2) }}</span>
                </div>
                <div class="flex justify-between text-xl font-bold text-slate-900 border-t pt-4 mt-4">
                  <span>Total</span>
                  <span>₱{{ total.toFixed(2) }}</span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      
      <!-- Success State -->
      <div v-else class="text-center py-20 px-4">
        <div class="w-24 h-24 bg-green-100 text-green-500 rounded-full flex items-center justify-center mx-auto mb-6">
          <svg xmlns="http://www.w3.org/2000/svg" width="48" height="48" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><path d="m9 11 3 3L22 4"/></svg>
        </div>
        <h1 class="text-4xl font-extrabold text-slate-900 mb-4 tracking-tight">Order Confirmed!</h1>
        <p class="text-xl text-slate-600 max-w-lg mx-auto mb-8">
          Thank you for choosing ChowStore. Your items will soon be on their way to your fluffy companion.
        </p>
        <button @click="$router.push('/')" class="inline-flex items-center justify-center px-8 py-4 bg-brand-600 hover:bg-brand-500 text-white rounded-full font-bold shadow transition-colors">
          Continue Shopping
        </button>
      </div>
    </div>
  </div>
</template>
