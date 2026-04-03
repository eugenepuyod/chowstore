<script setup>
import { RouterLink } from 'vue-router'
import { ref, onMounted, onUnmounted } from 'vue'
import ProductCard from '../components/ProductCard.vue'

const featuredProducts = ref([
  { id: 1, name: 'Chow Premium Kibble', category: 'Food', price: 45.99, image: '/images/premium_kibble.png', description: 'Grain-free formula for a shining coat.', badge: 'Best Seller' },
  { id: 3, name: 'Adorable Chow Puppy', category: 'Dogs', price: 1500.00, image: '/images/chow_puppy.png', description: 'Vet-checked and fully vaccinated puppy.' },
  { id: 5, name: 'Wild Alaskan Salmon Feast', category: 'Food', price: 55.00, image: '/images/wild-alaskan-salmon-feast.jpg', description: 'High protein nutrition tailored for Chows.', badge: 'Premium' },
])

const saleProducts = ref([
  { id: 15, name: 'Purebred Chow Chow Puppy', category: 'Dogs', price: 15000.00, image: '/images/puppy1.jpg', description: 'Fluffy, playful, and well-socialized. This puppy is vet-checked, vaccinated, and ready to bring joy to your home.', badge: 'Sale 10%' },
  { id: 16, name: 'Cute & Fluffy Chow Puppy', category: 'Dogs', price: 20000.00, image: '/images/puppy2.jpg', description: 'Vaccinated, healthy, and full of personality. Perfect companion for families and dog lovers.', badge: 'Sale 10%' },
  { id: 17, name: 'Beautiful Chow Chow Puppy for Sale', category: 'Dogs', price: 23000.00, image: '/images/puppy3.jpg', description: 'This adorable pup is vet-checked, fully vaccinated, and in excellent health. Known for its fluffy coat and loyal nature, the Chow Chow makes a wonderful companion for the right home.', badge: 'Sale 10%' },
  { id: 6, name: 'Playful Adult Chow', category: 'Dogs', price: 800.00, image: '/images/bigbrownchowleft.jpg', description: 'Trained adult Chow Chow seeking a loving home.', badge: 'Sale 20%e' },
  { id: 13, name: 'Big White Brown Chow', category: 'Dogs', price: 900.50, image: '/images/chow-chow-standing-in-park.jpg', description: 'Loyal and trained Chow Chow seeking a safe and affectionate home.', badge: 'Sale 10%' },
  { id: 14, name: 'The Ancient Origin Of Chow', category: 'Dogs', price: 1000.50, image: '/images/Anchient-origin-of-chowjpg.jpg', description: 'Gentle adult Chow Chow ready to join a loving family.', badge: 'Sale 10%' },
 
])

const partners = [
  { name: 'AKC Certified', icon: '🏆' },
  { name: 'Eco-Pet Alliance', icon: '🌿' },
  { name: 'Chow Rescue', icon: '🐾' },
  { name: 'Global Vet Org', icon: '🏥' },
  { name: 'Premium Feed Inc', icon: '🥩' },
]

const testimonials = [
  { name: 'Sarah Jenkins', role: 'Chow Owner', text: '"The premium kibble completely transformed my dog\'s coat. It is incredibly thick and fluffy! A must-have for any owner."', rating: 5, avatar: 'SJ' },
  { name: 'Mark D.', role: 'Adopter', text: '"Their rescue Chows are so well-cared for. I found my best friend here and couldn\'t be happier."', rating: 5, avatar: 'MD' },
  { name: 'Dr. Emily Chen', role: 'Veterinarian', text: '"I confidently recommend these vitamins to all my Chow Chow patients for superb joint health."', rating: 5, avatar: 'EC' },
]

const stats = ref([
  { label: 'Happy Furry Friends', target: 5000, current: 0, suffix: '+' },
  { label: 'Five Star Reviews', target: 2400, current: 0, suffix: '+' },
  { label: 'Premium Items', target: 150, current: 0, suffix: '+' },
  { label: 'Vet Awards', target: 24, current: 0, suffix: '' },
])

// Hero Slides Logic
const heroSlides = ref([
  {
    title: 'Premium Care for Fluffy Companions',
    titleGradient: 'Fluffy Companions',
    subtitle: 'Discover our curated selection of high-quality food, essential vitamins, and adorable Chow Chows.',
    image: '/images/chow_hero.png',
    tag: 'Premium Chow Care'
  },
  {
    title: 'Nutrition Tailored for Thick Coats',
    titleGradient: 'Thick Coats',
    subtitle: 'Our exclusive food lines guarantee a shiny, healthy, and vivid coat for your beloved dog, packed with wild-caught salmon and essential oils.',
    image: '/images/premium_kibble.png',
    tag: 'Specialized Diet'
  },
  {
    title: 'Find Your New Best Friend',
    titleGradient: 'Best Friend',
    subtitle: 'Provide a loving home to one of our incredibly sweet, vet-approved Chow Chow puppies.',
    image: '/images/chow_puppy.png',
    tag: 'Chow Companions'
  }
])
const activeSlide = ref(0)
let slideInterval = null

// Chatbot logic
const isChatOpen = ref(false)

const formStatus = ref(null)
const submitContactForm = () => {
  formStatus.value = 'Message sent successfully! We will get back to you shortly.'
  setTimeout(() => { formStatus.value = null }, 3000)
}

const addToCart = (product) => {
  alert(`Added ${product.name} to cart! (Mock)`)
}

// Logic for animating stats when they come into view
const statsSection = ref(null)
let statObserver = null
let animationHasRun = false

onMounted(() => {
  // Setup Hero Slide Interval
  slideInterval = setInterval(() => {
    activeSlide.value = (activeSlide.value + 1) % heroSlides.value.length
  }, 6000)

  // Setup Stats Observer
  statObserver = new IntersectionObserver((entries) => {
    // Only run animation when element is visible within threshold
    if (entries[0].isIntersecting && !animationHasRun) {
      animationHasRun = true
      
      const duration = 2500 // 2.5 seconds
      const frames = 60
      const frameDuration = duration / frames

      stats.value.forEach(stat => {
        let currentFrame = 0
        const counter = setInterval(() => {
          currentFrame++
          const progress = currentFrame / frames
          const easeOutProgress = 1 - Math.pow(1 - progress, 3)
          stat.current = Math.floor(stat.target * easeOutProgress)
          
          if (currentFrame >= frames) {
            stat.current = stat.target
            clearInterval(counter)
          }
        }, frameDuration)
      })
      statObserver.disconnect()
    }
  }, { threshold: 0.5 }) // triggers when at least 50% of stat block is visible

  if (statsSection.value) {
    statObserver.observe(statsSection.value)
  }
})

onUnmounted(() => {
  if (statObserver) statObserver.disconnect()
  if (slideInterval) clearInterval(slideInterval)
})
</script>

<template>
  <div class="flex flex-col w-full relative">
    
    <!-- AI Chatbot (Bottom Right) -->
    <div class="fixed bottom-6 right-6 z-50 flex flex-col items-end">
      <!-- Chat Window -->
      <transition name="chat-slide">
        <div v-show="isChatOpen" class="bg-white w-80 sm:w-96 rounded-2xl shadow-2xl border border-slate-200 overflow-hidden mb-4 flex flex-col h-[450px]">
          <!-- Header -->
          <div class="bg-slate-900 text-white p-4 flex justify-between items-center shadow-md z-10">
             <div class="flex items-center gap-3">
               <div class="w-8 h-8 rounded-full bg-brand-500 flex items-center justify-center text-lg">🤖</div>
               <div>
                  <div class="font-bold leading-tight">ChowBot AI</div>
                  <div class="text-[10px] text-brand-200 uppercase tracking-wider font-semibold flex items-center gap-1">
                    <span class="w-1.5 h-1.5 rounded-full bg-green-400 absolute animate-ping opacity-75"></span>
                    <span class="w-1.5 h-1.5 rounded-full bg-green-400"></span>
                    Online
                  </div>
               </div>
             </div>
             <button @click="isChatOpen = false" class="text-slate-400 hover:text-white transition-colors">
               <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M18 6 6 18"/><path d="m6 6 12 12"/></svg>
             </button>
          </div>
          <!-- Body -->
          <div class="flex-grow p-4 bg-slate-50 overflow-y-auto flex flex-col gap-3">
             <div class="text-center text-xs text-slate-400 mb-2 font-medium">Today at {{ new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' }) }}</div>
             
             <div class="flex gap-2 max-w-[85%]">
               <div class="w-6 h-6 rounded-full bg-slate-200 flex-shrink-0 flex items-center justify-center text-xs mt-1">🤖</div>
               <div class="bg-white border border-slate-100 p-3 rounded-2xl rounded-tl-sm text-sm text-slate-700 shadow-sm leading-relaxed">
                 Hi there! 👋 I'm ChowBot. I can help you find the perfect food, vitamins, or companion for your Chow Chow. What are you looking for today?
               </div>
             </div>

             <!-- Suggestion chips -->
             <div class="flex flex-wrap gap-2 ml-8 mt-1">
               <div class="bg-brand-50 hover:bg-brand-100 text-brand-700 border border-brand-200 text-xs px-3 py-1.5 rounded-full cursor-pointer transition-colors">Find best dog food</div>
               <div class="bg-brand-50 hover:bg-brand-100 text-brand-700 border border-brand-200 text-xs px-3 py-1.5 rounded-full cursor-pointer transition-colors">Track my order</div>
             </div>
          </div>
          <!-- Input -->
          <div class="p-3 bg-white border-t border-slate-100 flex gap-2">
            <input type="text" class="flex-grow bg-slate-50 border border-slate-200 rounded-full px-4 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-brand-500 transition-all font-medium text-slate-700" placeholder="Type your message...">
            <button class="w-10 h-10 bg-brand-600 text-white rounded-full flex items-center justify-center hover:bg-brand-500 transition-colors shadow-md flex-shrink-0">
              <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m22 2-7 20-4-9-9-4Z"/><path d="M22 2 11 13"/></svg>
            </button>
          </div>
        </div>
      </transition>

      <!-- Toggle Button -->
      <button @click="isChatOpen = !isChatOpen" class="w-16 h-16 bg-slate-900 text-white rounded-full shadow-2xl flex items-center justify-center hover:bg-brand-600 hover:scale-105 transition-all duration-300 border-[3px] border-white relative z-50">
        <transition name="fade" mode="out-in">
          <svg v-if="!isChatOpen" key="open" xmlns="http://www.w3.org/2000/svg" width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M7.9 20A9 9 0 1 0 4 16.1L2 22Z"/></svg>
          <svg v-else key="close" xmlns="http://www.w3.org/2000/svg" width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M18 6 6 18"/><path d="m6 6 12 12"/></svg>
        </transition>
        <!-- Notification dot -->
        <span v-if="!isChatOpen" class="absolute top-0 right-0 w-4 h-4 bg-rose-500 border-2 border-white rounded-full"></span>
      </button>
    </div>

    <!-- Hero Section -->
    <section class="relative w-full overflow-hidden bg-brand-50 lg:min-h-[95vh] flex items-center pt-4 pb-16 md:pt-8 md:pb-16">
      <div class="absolute inset-0 bg-gradient-to-br from-brand-100 to-white opacity-90 z-0"></div>
      
      <!-- Decorative circles -->
      <div class="absolute -top-40 -right-40 w-96 h-96 bg-brand-200 rounded-full mix-blend-multiply filter blur-3xl opacity-50 animate-blob"></div>
      <div class="absolute top-40 -left-20 w-72 h-72 bg-orange-200 rounded-full mix-blend-multiply filter blur-3xl opacity-50 animate-blob animation-delay-2000"></div>

      <!-- Fading Slides container -->
      <div class="relative z-10 max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 w-full pt-0 pb-16 md:pt-4 md:pb-12">
        <div class="grid w-full items-center relative" style="grid-template-areas: 'stack'; grid-template-columns: 1fr;">
          
          <div v-for="(slide, index) in heroSlides" :key="'slide-'+index" :class="[
                 'w-full flex flex-col md:flex-row items-center gap-8 md:gap-12 transition-all duration-[800ms] ease-in-out',
                 activeSlide === index ? 'opacity-100 z-10 translate-x-0' : 'opacity-0 z-0 pointer-events-none -translate-x-4'
               ]" style="grid-area: stack;">
            
            <div class="md:w-1/2 flex flex-col justify-center space-y-6 md:space-y-8 mt-2 md:mt-0 relative z-20 w-full">
              <div class="inline-flex items-center gap-2 px-3 py-1 rounded-full bg-brand-100/80 backdrop-blur border border-brand-200 w-max transform hover:scale-105 transition-transform cursor-pointer">
                <span class="flex h-2 w-2 rounded-full bg-brand-500 animate-pulse"></span>
                <span class="text-xs font-semibold text-brand-700 tracking-wide">{{ slide.tag }}</span>
              </div>
              <h1 class="text-5xl md:text-7xl font-extrabold text-slate-900 tracking-tight leading-[1.1]">
                {{ slide.title.replace(slide.titleGradient, '') }} <span class="bg-clip-text text-transparent bg-gradient-to-r from-brand-500 to-blue-600 drop-shadow-sm">{{ slide.titleGradient }}</span>
              </h1>
              <p class="text-lg md:text-xl text-slate-600 leading-relaxed max-w-lg">
                {{ slide.subtitle }}
              </p>
              <div class="flex flex-wrap gap-4 pt-2 md:pt-4">
                <RouterLink to="/shop" class="px-8 py-4 bg-brand-600 hover:bg-brand-500 text-white font-bold rounded-full shadow-[0_8px_30px_rgb(245,158,11,0.3)] hover:shadow-[0_8px_40px_rgb(245,158,11,0.4)] transition-all transform hover:-translate-y-1 w-full text-center sm:w-auto">
                  Shop Collection
                </RouterLink>
                <a href="#featured" class="px-8 py-4 bg-white text-slate-800 hover:bg-slate-50 font-bold rounded-full border border-slate-200 shadow-sm transition-all hover:border-slate-300 w-full text-center sm:w-auto">
                  View Features
                </a>
              </div>
            </div>
            
            <div class="md:w-1/2 relative w-full flex justify-center mt-6 md:mt-0">
              <div class="relative w-full aspect-square max-w-[16rem] sm:max-w-sm md:max-w-lg mx-auto">
                <div class="absolute inset-0 bg-gradient-to-tr from-brand-300 to-blue-200 rounded-full shadow-2xl opacity-20 transform -rotate-6 scale-105"></div>
                <div class="absolute inset-0 bg-white rounded-full p-4 md:p-8 shadow-2xl flex items-center justify-center overflow-hidden border-8 border-white group">
                   <img :src="slide.image" alt="Chow Chow Content" class="w-full h-full object-cover rounded-full group-hover:scale-110 transition-transform duration-700" />
                </div>
                
                <!-- Floating widgets -->
                <!-- Vet Approved: Centered top on mobile, Left offset on desktop -->
                <div class="absolute -top-6 left-1/2 -translate-x-1/2 md:top-20 md:-left-12 md:translate-x-0 md:left-auto bg-white p-3 md:p-4 rounded-xl md:rounded-2xl shadow-xl flex items-center gap-3 md:gap-4 animate-float border border-slate-100 z-30 w-max">
                  <div class="w-10 h-10 md:w-12 md:h-12 rounded-full bg-green-100 flex items-center justify-center text-green-600">
                    <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="md:w-6 md:h-6"><path d="m9 11 3 3L22 4"/><path d="M21 12v7a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h11"/></svg>
                  </div>
                  <div>
                    <div class="text-xs md:text-sm font-bold text-slate-800">Vet Approved</div>
                    <div class="text-[10px] md:text-xs text-slate-500">Safe for Chows</div>
                  </div>
                </div>

                <!-- 5.0 Rated: Centered bottom on mobile, Right offset on desktop -->
                <div class="absolute -bottom-6 left-1/2 -translate-x-1/2 md:bottom-20 md:-right-12 md:translate-x-0 md:left-auto bg-white p-3 md:p-4 rounded-xl md:rounded-2xl shadow-xl flex items-center gap-3 md:gap-4 animate-float animation-delay-2000 border border-slate-100 z-30 w-max">
                  <div class="w-10 h-10 md:w-12 md:h-12 rounded-full bg-brand-100 flex items-center justify-center text-brand-600">
                    <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="md:w-6 md:h-6"><polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"/></svg>
                  </div>
                  <div>
                    <div class="text-xs md:text-sm font-bold text-slate-800">5.0 Rated</div>
                    <div class="text-[10px] md:text-xs text-slate-500">10k+ Customers</div>
                  </div>
                </div>
              </div>
            </div>
            
          </div>
        </div>
      </div>
      
      <!-- Slide Indicators -->
      <div class="absolute bottom-6 md:bottom-8 left-0 right-0 z-20 flex justify-center gap-3">
        <button v-for="(slide, index) in heroSlides" :key="'indicator-'+index" @click="activeSlide = index" :class="[
          'w-3 h-3 rounded-full transition-all duration-300',
          activeSlide === index ? 'bg-brand-600 shadow-md w-8' : 'bg-slate-300 hover:bg-brand-400'
        ]"></button>
      </div>
    </section>

    <!-- Featured Products -->
    <section id="featured" class="py-24 bg-slate-50">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="flex flex-col md:flex-row justify-between items-start md:items-end mb-12 border-b border-slate-200 pb-6">
          <div>
            <span class="text-brand-600 font-bold tracking-wider uppercase text-sm mb-2 block">Premium Selection</span>
            <h2 class="text-3xl md:text-4xl font-extrabold text-slate-900">Featured</h2>
          </div>
          <RouterLink to="/shop" class="group flex items-center gap-2 text-brand-600 font-semibold hover:text-brand-800 transition-colors mt-4 md:mt-0">
            View All Store <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="transform group-hover:translate-x-1 transition-transform"><path d="M5 12h14"/><path d="m12 5 7 7-7 7"/></svg>
          </RouterLink>
        </div>
        
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
          <ProductCard 
            v-for="product in featuredProducts" 
            :key="product.id" 
            :product="product" 
            @add-to-cart="addToCart" 
          />
        </div>
      </div>
    </section>

    <!-- Sale Products -->
    <section class="py-24 bg-white">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="flex flex-col md:flex-row justify-between items-start md:items-end mb-12 border-b border-slate-200 pb-6">
          <div>
            <span class="text-rose-500 font-bold tracking-wider uppercase text-sm mb-2 block flex items-center gap-2">
              <span class="animate-pulse">🔴</span> Limited Time Offers
            </span>
            <h2 class="text-3xl md:text-4xl font-extrabold text-slate-900">On Sale Now</h2>
          </div>
          <RouterLink to="/shop" class="group flex items-center gap-2 text-slate-600 font-semibold hover:text-slate-900 transition-colors mt-4 md:mt-0">
            Shop All Deals <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="transform group-hover:translate-x-1 transition-transform"><path d="M5 12h14"/><path d="m12 5 7 7-7 7"/></svg>
          </RouterLink>
        </div>
        
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
          <ProductCard 
            v-for="product in saleProducts" 
            :key="product.id" 
            :product="product" 
            @add-to-cart="addToCart" 
          />
        </div>
      </div>
    </section>

    <!-- Promo Banner Section -->
    <section class="py-12 bg-white pb-12">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="relative rounded-[2.5rem] overflow-hidden bg-gradient-to-br from-brand-600 to-orange-500 shadow-2xl shadow-brand-500/20 flex flex-col md:flex-row items-center justify-between p-8 md:p-14 border border-brand-500/30">
          <!-- decorative bg -->
          <div class="absolute inset-0 bg-[url('data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI4IiBoZWlnaHQ9IjgiPjxyZWN0IHdpZHRoPSI4IiBoZWlnaHQ9IjgiIGZpbGw9IiNmZmYiIGZpbGwtb3BhY2l0eT0iMC4wNSIvPjwvc3ZnPg==')] opacity-40"></div>
          <div class="absolute top-0 right-0 w-64 h-64 bg-white opacity-10 rounded-full filter blur-3xl transform translate-x-1/2 -translate-y-1/2 cursor-none pointer-events-none"></div>
          
          <div class="relative z-10 md:w-2/3 text-center md:text-left mb-8 md:mb-0">
            <div class="inline-flex items-center gap-2 bg-white/10 text-brand-100 border border-white/20 px-4 py-1.5 rounded-full text-xs font-bold uppercase tracking-widest mb-6">
              <span class="w-1.5 h-1.5 rounded-full bg-amber-400 animate-pulse"></span>
              Special Promotion
            </div>
            <h2 class="text-3xl md:text-5xl font-extrabold text-white mb-5 tracking-tight leading-tight">Elevate Their Care</h2>
            <p class="text-brand-100 text-lg md:text-xl max-w-xl font-medium">Get 20% off your entire first order. Because your beautiful fluffy companion deserves nothing but premium care and comfort.</p>
          </div>
          
          <div class="relative z-10 flex flex-col items-center justify-center p-8 bg-white/10 backdrop-blur-md rounded-[2rem] border border-white/20 shadow-inner min-w-[280px] transform hover:scale-105 transition-transform cursor-pointer group">
            <span class="text-sm font-bold text-brand-200 uppercase tracking-widest mb-2 group-hover:text-white transition-colors">Use Checkout Code</span>
            <span class="text-4xl font-black text-white tracking-widest drop-shadow-md">CHOW20</span>
          </div>
        </div>
      </div>
    </section>

    <!-- Statistics Counters Section -->
    <section class="py-16 bg-white border-y border-slate-100 mb-12" ref="statsSection">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="grid grid-cols-2 md:grid-cols-4 gap-8 md:gap-12">
          <div v-for="stat in stats" :key="stat.label" class="text-center group">
            <div class="text-4xl md:text-5xl font-black text-slate-900 mb-2 flex items-center justify-center font-serif group-hover:-translate-y-1 transition-transform duration-300">
              <span class="bg-clip-text text-transparent bg-gradient-to-r from-brand-500 to-blue-600">{{ stat.current }}</span>
              <span class="text-brand-500 ml-0.5">{{ stat.suffix }}</span>
            </div>
            <div class="text-sm md:text-base text-slate-500 font-semibold tracking-wide uppercase">{{ stat.label }}</div>
          </div>
        </div>
      </div>
    </section>

    <!-- Categories Section -->
    <section class="py-24 bg-slate-900 text-white rounded-[3rem] mx-4 sm:mx-8 lg:mx-12 mb-12 relative overflow-hidden">
      <!-- Decor -->
      <div class="absolute top-0 right-0 w-64 h-64 bg-brand-500 opacity-20 rounded-full filter blur-[100px]"></div>
      <div class="absolute bottom-0 left-0 w-64 h-64 bg-slate-500 opacity-20 rounded-full filter blur-[100px]"></div>

      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 relative z-10">
        <div class="text-center mb-16">
          <h2 class="text-3xl md:text-5xl font-bold mb-6">Explore By Category</h2>
          <p class="text-slate-400 max-w-2xl mx-auto text-lg">From nutrition to comfort, explore our categories tailored for your Chow Chow's unique requirements.</p>
        </div>
        
        <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
          <RouterLink to="/shop" class="group relative rounded-3xl overflow-hidden bg-white/5 border border-white/10 hover:border-white/20 p-8 text-center transition-all duration-300 hover:-translate-y-2 hover:bg-white/10 backdrop-blur-sm">
            <div class="w-24 h-24 mx-auto mb-6 bg-gradient-to-br from-brand-400 to-orange-600 rounded-2xl shadow-lg flex items-center justify-center group-hover:scale-110 group-hover:rotate-3 transition-transform duration-300">
               <span class="text-4xl filter drop-shadow">🥩</span>
            </div>
            <h3 class="text-2xl font-bold mb-3">Premium Food</h3>
            <p class="text-slate-400 mb-6">Formulated specifically for thick coats and robust energy.</p>
            <div class="text-brand-400 font-semibold inline-flex items-center gap-2 group-hover:text-brand-300 transition-colors">
              Explore Collection <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="group-hover:translate-x-1 transition-transform"><path d="M5 12h14"/><path d="m12 5 7 7-7 7"/></svg>
            </div>
          </RouterLink>
          
          <RouterLink to="/shop" class="group relative rounded-3xl overflow-hidden bg-white/5 border border-white/10 hover:border-white/20 p-8 text-center transition-all duration-300 hover:-translate-y-2 hover:bg-white/10 backdrop-blur-sm">
            <div class="w-24 h-24 mx-auto mb-6 bg-gradient-to-br from-rose-400 to-pink-600 rounded-2xl shadow-lg flex items-center justify-center group-hover:scale-110 group-hover:-rotate-3 transition-transform duration-300">
               <span class="text-4xl filter drop-shadow">💊</span>
            </div>
            <h3 class="text-2xl font-bold mb-3">Vital Vitamins</h3>
            <p class="text-slate-400 mb-6">Support joint health, digestion, and brilliant coat shine.</p>
            <div class="text-rose-400 font-semibold inline-flex items-center gap-2 group-hover:text-rose-300 transition-colors">
              Explore Collection <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="group-hover:translate-x-1 transition-transform"><path d="M5 12h14"/><path d="m12 5 7 7-7 7"/></svg>
            </div>
          </RouterLink>
          
          <RouterLink to="/shop" class="group relative rounded-3xl overflow-hidden bg-white/5 border border-white/10 hover:border-white/20 p-8 text-center transition-all duration-300 hover:-translate-y-2 hover:bg-white/10 backdrop-blur-sm">
            <div class="w-24 h-24 mx-auto mb-6 bg-gradient-to-br from-amber-400 to-orange-600 rounded-2xl shadow-lg flex items-center justify-center group-hover:scale-110 group-hover:rotate-3 transition-transform duration-300">
               <span class="text-4xl filter drop-shadow">🐕</span>
            </div>
            <h3 class="text-2xl font-bold mb-3">Adorable Dogs</h3>
            <p class="text-slate-400 mb-6">Find your new best friend from our vet-checked rescues.</p>
            <div class="text-amber-400 font-semibold inline-flex items-center gap-2 group-hover:text-amber-300 transition-colors">
              Explore Collection <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="group-hover:translate-x-1 transition-transform"><path d="M5 12h14"/><path d="m12 5 7 7-7 7"/></svg>
            </div>
          </RouterLink>
        </div>
      </div>
    </section>

    <!-- Testimonials Section -->
    <section class="py-24 bg-brand-50 border-t border-brand-100 relative overflow-hidden">
      <!-- Decor -->
      <div class="absolute top-10 right-10 w-40 h-40 bg-white rounded-full opacity-50 blur-xl"></div>
      
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 relative z-10">
        <div class="text-center mb-16">
          <h2 class="text-3xl md:text-4xl font-extrabold text-slate-900 mb-4">Loved by Chow Owners</h2>
          <p class="text-slate-600 max-w-xl mx-auto">See what our community has to say about our premium quality products.</p>
        </div>
        
        <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
          <div v-for="t in testimonials" :key="t.name" class="bg-white p-8 rounded-3xl shadow-[0_4px_20px_-10px_rgba(0,0,0,0.1)] border border-slate-100 relative group hover:-translate-y-2 transition-transform duration-300">
            
            <div class="absolute -top-5 -right-2 text-6xl text-brand-100 font-serif opacity-50 group-hover:opacity-100 group-hover:-translate-y-1 transition-all">"</div>
            
            <div class="text-amber-400 mb-6 flex gap-1 text-sm">
              <span v-for="i in t.rating" :key="i">⭐</span>
            </div>
            
            <p class="text-slate-700 font-medium italic mb-8 relative z-10 leading-relaxed">
              {{ t.text }}
            </p>
            
            <div class="flex items-center gap-4 border-t border-slate-50 pt-6 mt-auto">
              <div class="w-12 h-12 rounded-full bg-slate-900 text-white flex items-center justify-center font-bold text-lg shadow-md">{{ t.avatar }}</div>
              <div>
                <div class="font-bold text-slate-900">{{ t.name }}</div>
                <div class="text-xs text-brand-600 font-semibold uppercase tracking-wider">{{ t.role }}</div>
              </div>
            </div>
            
          </div>
        </div>
      </div>
    </section>

    <!-- Partners Section (Auto-sliding) -->
    <section class="py-16 bg-white overflow-hidden border-y border-slate-100 relative w-full">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 mb-10">
        <p class="text-center text-sm font-bold text-slate-400 uppercase tracking-widest">Trusted By Industry Leaders</p>
      </div>
      
      <!-- Scrolling Wrapper -->
      <div class="relative w-full overflow-hidden flex bg-white py-6">
        <!-- We duplicate the items a few times to make a seamless loop -->
        <div class="animate-marquee flex whitespace-nowrap items-center gap-20 md:gap-32 opacity-60">
          <div v-for="(partner, i) in [...partners, ...partners, ...partners]" :key="i" class="flex items-center gap-5 grayscale hover:grayscale-0 transition-all duration-300 min-w-max">
             <span class="text-5xl md:text-6xl">{{ partner.icon }}</span>
             <span class="font-bold text-slate-800 text-2xl md:text-3xl">{{ partner.name }}</span>
          </div>
        </div>
      </div>
    </section>

    <!-- Contact Form Section -->
    <section class="py-24 bg-white relative">
      <div class="absolute inset-0 bg-gradient-to-b from-slate-50 to-white z-0"></div>
      
      <div class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8 relative z-10">
        <div class="bg-brand-50 rounded-[3rem] p-8 md:p-14 relative overflow-hidden shadow-xl shadow-brand-100/30 border border-brand-100">
          <!-- decorative abstract bg -->
          <div class="absolute right-0 top-0 -mr-20 -mt-20 w-64 h-64 bg-gradient-to-bl from-brand-200 to-transparent rounded-full opacity-60"></div>
          <div class="absolute left-0 bottom-0 -ml-20 -mb-20 w-64 h-64 bg-gradient-to-tr from-brand-200 to-transparent rounded-full opacity-60"></div>
          
          <div class="relative z-10 mx-auto text-center mb-10">
            <h2 class="text-3xl md:text-5xl font-extrabold text-slate-900 mb-4 tracking-tight">Get In Touch</h2>
            <p class="text-slate-600 text-lg">Have questions about our Chow-approved products? We'd love to help.</p>
          </div>
          
          <form @submit.prevent="submitContactForm" class="relative z-10 max-w-2xl mx-auto space-y-6">
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
              <div>
                <label for="name" class="block text-sm font-semibold text-slate-700 mb-2">Full Name</label>
                <input type="text" id="name" required class="w-full px-5 py-4 rounded-2xl border-0 ring-1 ring-inset ring-slate-200 bg-white focus:outline-none focus:ring-2 focus:ring-inset focus:ring-brand-500 transition-all shadow-sm" placeholder="Eugene Doe">
              </div>
              <div>
                <label for="email" class="block text-sm font-semibold text-slate-700 mb-2">Email Address</label>
                <input type="email" id="email" required class="w-full px-5 py-4 rounded-2xl border-0 ring-1 ring-inset ring-slate-200 bg-white focus:outline-none focus:ring-2 focus:ring-inset focus:ring-brand-500 transition-all shadow-sm" placeholder="eugene@chowstore.com">
              </div>
            </div>
            <div>
              <label for="subject" class="block text-sm font-semibold text-slate-700 mb-2">Subject</label>
              <input type="text" id="subject" class="w-full px-5 py-4 rounded-2xl border-0 ring-1 ring-inset ring-slate-200 bg-white focus:outline-none focus:ring-2 focus:ring-inset focus:ring-brand-500 transition-all shadow-sm" placeholder="How can we help?">
            </div>
            <div>
              <label for="message" class="block text-sm font-semibold text-slate-700 mb-2">Message</label>
              <textarea id="message" rows="5" required class="w-full px-5 py-4 rounded-2xl border-0 ring-1 ring-inset ring-slate-200 bg-white focus:outline-none focus:ring-2 focus:ring-inset focus:ring-brand-500 transition-all shadow-sm resize-none" placeholder="Write your message here..."></textarea>
            </div>
            
            <button type="submit" class="w-full py-5 bg-slate-900 text-white font-bold rounded-2xl hover:bg-brand-600 transition-all duration-300 shadow-xl shadow-slate-900/20 hover:shadow-brand-500/30 flex justify-center items-center gap-2 text-lg group">
              Send Message
              <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="transform group-hover:translate-x-1 group-hover:-translate-y-1 transition-transform"><path d="m22 2-7 20-4-9-9-4Z"/><path d="M22 2 11 13"/></svg>
            </button>
            
            <!-- Success Message Toast/Box -->
            <transition name="fade">
              <div v-if="formStatus" class="mt-6 p-5 bg-green-50 border border-green-200 text-green-700 rounded-2xl text-center text-base font-medium flex items-center justify-center gap-3">
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="text-green-600"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><path d="m9 11 3 3L22 4"/></svg>
                {{ formStatus }}
              </div>
            </transition>
          </form>
        </div>
      </div>
    </section>
  </div>
</template>

<style scoped>
.animate-blob {
  animation: blob 7s infinite;
}
.animation-delay-2000 {
  animation-delay: 2s;
}
@keyframes blob {
  0% { transform: translate(0px, 0px) scale(1); }
  33% { transform: translate(30px, -50px) scale(1.1); }
  66% { transform: translate(-20px, 20px) scale(0.9); }
  100% { transform: translate(0px, 0px) scale(1); }
}
.animate-bounce-slow {
  animation: bounce 3s infinite;
}

/* Auto-sliding marquee animation */
.animate-marquee {
  width: max-content;
  animation: marquee 30s linear infinite;
}
.animate-marquee:hover {
  animation-play-state: paused;
}
@keyframes marquee {
  0% {
    transform: translateX(0%);
  }
  100% {
    transform: translateX(-33.333333%);
  }
}

/* Hero Slides Fade Transition */
.hero-fade-enter-active,
.hero-fade-leave-active {
  transition: opacity 1s ease, transform 1s ease;
}
.hero-fade-enter-from {
  opacity: 0;
  transform: scale(0.98);
}
.hero-fade-leave-to {
  opacity: 0;
  transform: scale(1.02);
}

/* Vue transitions for form status */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease, transform 0.3s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateY(10px);
}

/* Vue transitions for Chat Slide */
.chat-slide-enter-active,
.chat-slide-leave-active {
  transition: opacity 0.3s ease, transform 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  transform-origin: bottom right;
}
.chat-slide-enter-from,
.chat-slide-leave-to {
  opacity: 0;
  transform: scale(0.8) translateY(20px);
}
</style>
