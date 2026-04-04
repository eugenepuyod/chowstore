<template>
  <div class="min-h-screen flex flex-col font-sans">
    <!-- Navbar -->
    <header class="sticky top-0 z-50 bg-white/80 backdrop-blur-md border-b border-white/20 shadow-sm">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="flex justify-between items-center h-16">
          <div class="flex-shrink-0 flex items-center gap-2 cursor-pointer" @click="$router.push('/')">
            <div class="w-8 h-8 rounded-full bg-brand-500 flex items-center justify-center text-white font-bold shadow-lg">
              C
            </div>
            <span class="font-bold text-xl text-brand-900 tracking-tight">ChowStore</span>
          </div>
          <nav class="hidden md:flex space-x-8">
            <RouterLink to="/" class="text-slate-600 hover:text-brand-600 font-medium transition-colors" active-class="text-brand-600">Home</RouterLink>
            <RouterLink to="/shop" class="text-slate-600 hover:text-brand-600 font-medium transition-colors" active-class="text-brand-600">Shop</RouterLink>
            <RouterLink to="/about" class="text-slate-600 hover:text-brand-600 font-medium transition-colors" active-class="text-brand-600">About Us</RouterLink>
            <RouterLink to="/contact" class="text-slate-600 hover:text-brand-600 font-medium transition-colors" active-class="text-brand-600">Contact</RouterLink>
          </nav>
          <div class="flex items-center">
            <RouterLink to="/cart" class="relative p-2 text-slate-600 hover:text-brand-600 transition-colors mr-2 md:mr-0">
              <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-shopping-cart"><circle cx="8" cy="21" r="1"/><circle cx="19" cy="21" r="1"/><path d="M2.05 2.05h2l2.66 12.42a2 2 0 0 0 2 1.58h9.78a2 2 0 0 0 1.95-1.57l1.65-7.43H5.12"/></svg>
              <span class="absolute top-0 right-0 inline-flex items-center justify-center px-2 py-1 text-xs font-bold leading-none text-white transform translate-x-1/2 -translate-y-1/2 bg-rose-500 rounded-full">3</span>
            </RouterLink>
            <RouterLink to="/checkout" class="hidden md:inline-flex ml-4 md:ml-6 items-center justify-center px-4 py-2 border border-transparent rounded-full shadow-sm text-sm font-medium text-white bg-brand-600 hover:bg-brand-500 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-brand-500 transition-colors">
              Checkout
            </RouterLink>

            <!-- Mobile Menu Toggle Button -->
            <button @click="isMobileMenuOpen = !isMobileMenuOpen" class="md:hidden p-2 text-slate-600 hover:text-brand-600 transition-colors focus:outline-none ml-2 bg-slate-100 rounded-lg">
              <svg v-if="!isMobileMenuOpen" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="4" x2="20" y1="12" y2="12"/><line x1="4" x2="20" y1="6" y2="6"/><line x1="4" x2="20" y1="18" y2="18"/></svg>
              <svg v-else xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M18 6 6 18"/><path d="m6 6 12 12"/></svg>
            </button>
          </div>
        </div>
      </div>

      <!-- Mobile Dropdown Menu -->
      <transition name="mobile-menu">
        <div v-show="isMobileMenuOpen" class="md:hidden bg-white border-b border-slate-200 absolute w-full left-0 top-16 shadow-xl z-40 overflow-hidden">
          <div class="px-4 py-4 flex flex-col space-y-2">
            <RouterLink @click="isMobileMenuOpen = false" to="/" class="block px-4 py-3 text-lg font-semibold text-slate-800 hover:bg-brand-50 hover:text-brand-600 rounded-xl transition-colors">Home</RouterLink>
            <RouterLink @click="isMobileMenuOpen = false" to="/shop" class="block px-4 py-3 text-lg font-semibold text-slate-800 hover:bg-brand-50 hover:text-brand-600 rounded-xl transition-colors">Shop</RouterLink>
            <RouterLink @click="isMobileMenuOpen = false" to="/about" class="block px-4 py-3 text-lg font-semibold text-slate-800 hover:bg-brand-50 hover:text-brand-600 rounded-xl transition-colors">About Us</RouterLink>
            <RouterLink @click="isMobileMenuOpen = false" to="/contact" class="block px-4 py-3 text-lg font-semibold text-slate-800 hover:bg-brand-50 hover:text-brand-600 rounded-xl transition-colors">Contact</RouterLink>
            <div class="border-t border-slate-100 my-2 pt-4">
              <RouterLink @click="isMobileMenuOpen = false" to="/checkout" class="w-full flex justify-center items-center px-4 py-3.5 border border-transparent rounded-xl shadow-sm text-base font-bold text-white bg-brand-600 hover:bg-brand-500 transition-colors">
                Proceed to Checkout
              </RouterLink>
            </div>
          </div>
        </div>
      </transition>
    </header>

    <!-- Main Content -->
    <main class="flex-grow flex flex-col">
      <RouterView />
    </main>

    <!-- Footer -->
    <footer class="bg-slate-900 text-slate-300 py-16 border-t border-slate-800 mt-auto">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-12 lg:gap-8">
        
        <!-- 1: About -->
        <div>
          <span class="text-white font-bold text-2xl mb-6 block tracking-tight">ChowStore</span>
          <p class="text-sm text-slate-400 mb-6 leading-relaxed">Premium quality products curated exclusively for your beloved Chow Chow. We prioritize their health, happiness, and unparalleled comfort.</p>
          <div class="flex gap-3">
            <span class="w-9 h-9 rounded-full bg-slate-800 flex items-center justify-center hover:bg-brand-600 text-white transition-colors cursor-pointer border border-slate-700 shadow-sm font-semibold text-xs"><a href="#" class="social-icon"><Facebook /></a></span>
            <span class="w-9 h-9 rounded-full bg-slate-800 flex items-center justify-center hover:bg-brand-600 text-white transition-colors cursor-pointer border border-slate-700 shadow-sm font-semibold text-xs"><a href="#" class="social-icon"><Instagram /></a></span>
            <span class="w-9 h-9 rounded-full bg-slate-800 flex items-center justify-center hover:bg-brand-600 text-white transition-colors cursor-pointer border border-slate-700 shadow-sm font-semibold text-xs"><a href="#" class="social-icon"><Twitter /></a></span>
          </div>
        </div>
        
        <!-- 2: Quick Links -->
        <div class="lg:pl-8">
          <h3 class="text-white font-bold text-lg mb-6">Quick Links</h3>
          <ul class="space-y-4 text-sm">
            <li><RouterLink to="/" class="hover:text-brand-400 transition-colors flex items-center gap-2"><span class="text-slate-600 text-lg leading-none">&rsaquo;</span> Home</RouterLink></li>
            <li><RouterLink to="/shop" class="hover:text-brand-400 transition-colors flex items-center gap-2"><span class="text-slate-600 text-lg leading-none">&rsaquo;</span> Shop Collection</RouterLink></li>
            <li><RouterLink to="/about" class="hover:text-brand-400 transition-colors flex items-center gap-2"><span class="text-slate-600 text-lg leading-none">&rsaquo;</span> About Us</RouterLink></li>
            <li><RouterLink to="/cart" class="hover:text-brand-400 transition-colors flex items-center gap-2"><span class="text-slate-600 text-lg leading-none">&rsaquo;</span> Shopping Cart</RouterLink></li>
            <li><RouterLink to="/contact" class="hover:text-brand-400 transition-colors flex items-center gap-2"><span class="text-slate-600 text-lg leading-none">&rsaquo;</span> Contact Support</RouterLink></li>
          </ul>
        </div>
        
        <!-- 3: Popular Products -->
        <div class="lg:pl-4">
          <h3 class="text-white font-bold text-lg mb-6">Popular Collection</h3>
          <ul class="space-y-5 text-sm">
            <li>
              <RouterLink to="/shop" class="group flex gap-4 hover:text-white transition-colors">
                <div class="w-14 h-14 rounded-xl overflow-hidden bg-slate-800 border border-slate-700 flex-shrink-0">
                  <img src="/images/premium_kibble.png" class="w-full h-full object-cover group-hover:scale-110 transition-transform duration-500">
                </div>
                <div class="flex flex-col justify-center">
                  <span class="group-hover:text-brand-400 transition-colors font-medium text-slate-200">Chow Premium Kibble</span>
                  <span class="text-brand-500 font-semibold mt-1">₱45.99</span>
                </div>
              </RouterLink>
            </li>
            <li>
              <RouterLink to="/shop" class="group flex gap-4 hover:text-white transition-colors">
                <div class="w-14 h-14 rounded-xl overflow-hidden bg-slate-800 border border-slate-700 flex-shrink-0">
                  <img src="/images/chow_puppy.png" class="w-full h-full object-cover group-hover:scale-110 transition-transform duration-500">
                </div>
                <div class="flex flex-col justify-center">
                  <span class="group-hover:text-brand-400 transition-colors font-medium text-slate-200">Adorable Chow Puppy</span>
                  <span class="text-brand-500 font-semibold mt-1">₱1500.00</span>
                </div>
              </RouterLink>
            </li>
            <li>
              <RouterLink to="/shop" class="group flex gap-4 hover:text-white transition-colors">
                <div class="w-14 h-14 rounded-xl overflow-hidden bg-slate-800 border border-slate-700 flex-shrink-0">
                  <img src="/images/wild-alaskan-salmon-feast.jpg" class="w-full h-full object-cover group-hover:scale-110 transition-transform duration-500">
                </div>
                <div class="flex flex-col justify-center">
                  <span class="group-hover:text-brand-400 transition-colors font-medium text-slate-200">Wild Alaskan Salmon Feast</span>
                  <span class="text-brand-500 font-semibold mt-1">₱55.00</span>
                </div>
              </RouterLink>
            </li>
          </ul>
        </div>
        
        <!-- 4: Contact Us & Payment -->
        <div>
          <h3 class="text-white font-bold text-lg mb-6">Contact Info</h3>
          <ul class="space-y-4 text-sm mb-8 relative">
            <li class="flex items-start gap-3">
              <MapPin class="icon-sm" />
              <span class="leading-relaxed">Anami Homes North, Jugan<br>Consolacion, Philippines</span>
            </li>
            <li class="flex items-center gap-3">
              <Phone class="icon-sm" />
              <span class="font-medium">+63 912 345 6789</span>
            </li>
            <li class="flex items-center gap-3">
              <Mail class="icon-sm" />
              <a href="mailto:eugene@chowstore.com" class="hover:text-brand-400 transition-colors font-medium">eugene@chowstore.com</a>
            </li>
          </ul>
          
          <h3 class="text-white font-bold text-sm mb-4 uppercase tracking-wider">Secure Checkout</h3>
          <div class="flex gap-2.5">
             <div class="w-11 h-7 bg-[white] rounded flex items-center justify-center text-[10px] font-bold border border-slate-700 text-slate-300"><img src="/images/visa.png" width="50" height="50" /></div>
             <div class="w-11 h-7 bg-[white] rounded flex items-center justify-center text-[10px] font-bold border border-slate-700 text-slate-300"><img src="/images/mastercard.png" width="50" height="50" /></div>
             <div class="w-11 h-7 bg-[white] rounded flex items-center justify-center text-[10px] font-bold border border-slate-700 text-slate-300"><img src="/images/bdo.png" width="50" height="50" /></div>
             <div class="w-11 h-7 bg-[white] rounded flex items-center justify-center text-[10px] font-bold border border-slate-700 text-slate-300"><img src="/images/bpi.png" width="50" height="50" /></div>
             <div class="w-11 h-7 bg-[white] rounded flex items-center justify-center text-[9px] font-bold border border-slate-700 text-slate-300"><img src="/images/paypal.png" width="50" height="50" /></div>
             <div class="w-11 h-7 bg-[white] rounded flex items-center justify-center text-[9px] font-bold border border-slate-700 text-slate-300"><img src="/images/gcash.png" width="50" height="50" /></div>
          </div>
        </div>
        
      </div>
      
      <!-- Footer Bottom -->
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 mt-16 pt-8 border-t border-slate-800 flex flex-col md:flex-row items-center justify-between text-sm text-slate-500">
        <p>&copy; 2026 ChowStore. All rights reserved.</p>
        <div class="flex gap-6 mt-4 md:mt-0">
          <a href="#" class="hover:text-white transition-colors">Privacy Policy</a>
          <a href="#" class="hover:text-white transition-colors">Terms of Service</a>
          <a href="#" class="hover:text-white transition-colors">Shipping Info</a>
        </div>
      </div>
    </footer>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { RouterLink, RouterView } from 'vue-router'
import { MapPin, Facebook, Instagram, Twitter, Phone, Mail } from 'lucide-vue-next'

const isMobileMenuOpen = ref(false)
</script>

<style>
.mobile-menu-enter-active,
.mobile-menu-leave-active {
  transition: all 0.3s ease-in-out;
  transform-origin: top;
}
.mobile-menu-enter-from,
.mobile-menu-leave-to {
  opacity: 0;
  transform: translateY(-10px);
}
</style>
