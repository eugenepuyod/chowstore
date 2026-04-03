<script setup>
import { ref, onMounted, watch, computed } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import { products } from '../data/products'

const route = useRoute()
const router = useRouter()

const product = ref(null)
const relatedProducts = ref([])

onMounted(() => {
  loadProduct(route.params.id)
})

watch(() => route.params.id, (newId) => {
  loadProduct(newId)
})

const loadProduct = (idParam) => {
  const id = parseInt(idParam)
  const found = products.find(p => p.id === id)
  if (found) {
    product.value = found
    selectedImage.value = found.image
    // grab related max 3
    relatedProducts.value = products.filter(p => p.category === found.category && p.id !== id).slice(0, 3)
    window.scrollTo({ top: 0, behavior: 'smooth' })
  } else {
    product.value = null
  }
}

// Image Gallery State
const selectedImage = ref('')
const sliderRef = ref(null)

const scrollLeft = () => {
  if (sliderRef.value) {
    sliderRef.value.scrollBy({ left: -150, behavior: 'smooth' })
  }
}

const scrollRight = () => {
  if (sliderRef.value) {
    sliderRef.value.scrollBy({ left: 150, behavior: 'smooth' })
  }
}

// Magnifier State
const isZooming = ref(false)
const zoomStyle = ref({})

const galleryImages = computed(() => {
  if (product.value.images && product.value.images.length > 0) return product.value.images;
  return [
    product.value.image,
    '/images/chow_adult.png', 
    '/images/chow_puppy.png',
    '/images/premium_kibble.png'
  ]
})

const handleMouseMove = (e) => {
  const { left, top, width, height } = e.target.getBoundingClientRect()
  const x = ((e.clientX - left) / width) * 100
  const y = ((e.clientY - top) / height) * 100
  zoomStyle.value = {
    transformOrigin: `${x}% ${y}%`,
    transform: 'scale(2.5)' // Increase scale for stronger magnification
  }
}

const goBack = () => {
  router.push('/shop')
}
const addToCart = () => {
  alert(`Added ${product.value.name} to cart! (Mock)`)
}
</script>

<template>
  <div class="bg-slate-50 min-h-[90vh] py-12">
    <div class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8">
      
      <!-- Loading/Error -->
      <div v-if="!product" class="text-center py-20 flex flex-col items-center justify-center">
        <div class="w-16 h-16 bg-slate-200 rounded-full flex items-center justify-center mb-4 text-2xl">👀</div>
        <h2 class="text-2xl font-bold text-slate-800">Product not found.</h2>
        <p class="text-slate-500 max-w-sm mt-2 mb-6">We couldn't locate the product you were looking for. It might have been moved or removed.</p>
        <button @click="goBack" class="px-6 py-3 bg-brand-600 hover:bg-brand-500 transition-colors text-white font-bold rounded-xl shadow-sm">Return to Shop Collection</button>
      </div>

      <div v-else>
        <!-- Nav Breadcrumbs -->
        <button @click="goBack" class="flex items-center gap-2 text-slate-500 hover:text-brand-600 font-bold tracking-wide uppercase text-sm mb-8 transition-colors bg-white px-4 py-2 border border-slate-200 rounded-full shadow-sm">
          <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m15 18-6-6 6-6"/></svg>
          Back to Collection
        </button>

        <!-- Product Hero -->
        <div class="bg-white rounded-[2rem] md:rounded-[3rem] p-6 lg:p-12 shadow-[0_4px_25px_-10px_rgba(0,0,0,0.06)] border border-slate-100 flex flex-col lg:flex-row gap-12 lg:gap-16 mb-16 relative overflow-hidden">
          
          <div class="absolute top-0 right-0 w-64 h-64 bg-brand-100 rounded-full opacity-50 filter blur-[80px] z-0"></div>

          <!-- Left Column: Image Gallery with Magnifier -->
          <div class="w-full lg:w-1/2 flex flex-col gap-4 z-10 w-full overflow-hidden">
            <!-- Main Magnifiable Image -->
            <div 
              class="relative bg-slate-50 rounded-3xl aspect-square sm:aspect-auto sm:h-[500px] overflow-hidden border border-slate-100 shadow-inner cursor-zoom-in group/zoom"
              @mouseenter="isZooming = true"
              @mouseleave="isZooming = false; zoomStyle = {}"
              @mousemove="handleMouseMove"
            >
              <div v-if="product.badge" class="absolute top-6 left-6 bg-rose-500 text-white text-sm font-bold px-4 py-1.5 rounded-full z-20 shadow-sm">{{ product.badge }}</div>
              
              <img 
                :src="selectedImage" 
                :alt="product.name" 
                :style="isZooming ? zoomStyle : {}"
                class="w-full h-full object-cover transition-transform duration-[50ms] ease-out will-change-transform"
              >
              
              <!-- Zoom hint overlay (shown before hover) -->
              <div v-if="!isZooming" class="absolute bottom-4 right-4 bg-slate-900/50 backdrop-blur text-white text-xs font-bold px-3 py-1.5 rounded-full z-10 flex items-center gap-1 opacity-0 group-hover/zoom:opacity-100 transition-opacity">
                <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="11" cy="11" r="8"/><path d="m21 21-4.3-4.3"/><line x1="11" x2="11" y1="8" y2="14"/><line x1="8" x2="14" y1="11" y2="11"/></svg>
                Hover to Zoom
              </div>
            </div>
            
            <!-- Thumbnail Slider with Arrows -->
            <div class="relative group/slider mt-2 px-6">
              
              <button @click="scrollLeft" class="absolute left-0 top-1/2 -translate-y-1/2 w-10 h-10 rounded-full bg-white shadow-[0_4px_15px_rgba(0,0,0,0.1)] border border-slate-100 flex items-center justify-center text-slate-600 hover:text-brand-600 hover:scale-110 transition-all z-20 opacity-0 group-hover/slider:opacity-100 focus:opacity-100">
                <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m15 18-6-6 6-6"/></svg>
              </button>

              <div ref="sliderRef" class="flex flex-nowrap gap-4 overflow-x-auto pb-2 pt-2 snap-x scroll-smooth [&::-webkit-scrollbar]:hidden [-ms-overflow-style:none] [scrollbar-width:none]">
                <button 
                  v-for="(img, idx) in galleryImages" 
                  :key="idx" 
                  @click="selectedImage = img"
                  :class="[
                    'w-20 h-20 sm:w-24 sm:h-24 flex-shrink-0 flex items-center justify-center rounded-2xl overflow-hidden border-2 transition-all p-1 snap-start relative',
                    selectedImage === img ? 'border-brand-500 opacity-100 shadow-md bg-white scale-100' : 'border-transparent opacity-70 hover:opacity-100 bg-slate-100 scale-95 hover:scale-100 hover:shadow-sm'
                  ]"
                >
                  <img :src="img" class="w-full h-full object-cover rounded-xl shadow-inner pointer-events-none">
                </button>
              </div>

              <button @click="scrollRight" class="absolute right-0 top-1/2 -translate-y-1/2 w-10 h-10 rounded-full bg-white shadow-[0_4px_15px_rgba(0,0,0,0.1)] border border-slate-100 flex items-center justify-center text-slate-600 hover:text-brand-600 hover:scale-110 transition-all z-20 opacity-0 group-hover/slider:opacity-100 focus:opacity-100">
                <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m9 18 6-6-6-6"/></svg>
              </button>
              
            </div>
          </div>

          <!-- Details -->
          <div class="w-full lg:w-1/2 flex flex-col justify-center z-10 py-4">
            <span class="text-brand-600 font-bold uppercase tracking-widest text-sm mb-3 block">{{ product.category }}</span>
            <h1 class="text-4xl lg:text-5xl font-extrabold text-slate-900 leading-tight mb-4 tracking-tight">{{ product.name }}</h1>
            <div class="text-4xl font-black text-slate-800 mb-8">₱{{ product.price.toFixed(2) }}</div>
            
            <p class="text-lg text-slate-600 leading-relaxed mb-8 border-b border-slate-100 pb-8 mr-8">
               {{ product.description }}
            </p>

            <div v-if="product.tags" class="flex gap-2 flex-wrap mb-8">
               <span v-for="tag in product.tags" :key="tag" class="px-4 py-1.5 bg-slate-100 border border-slate-200 text-slate-600 text-xs uppercase tracking-widest font-bold rounded-lg shadow-sm">
                 #{{ tag }}
               </span>
            </div>
            
            <div class="flex gap-4">
              <!-- Mock Quantity -->
              <div class="flex items-center bg-white rounded-2xl p-1 border border-slate-200 shadow-sm">
                <button class="w-12 h-12 rounded-xl flex items-center justify-center text-slate-500 hover:bg-slate-100 hover:text-slate-900 transition-colors text-xl font-medium">-</button>
                <div class="w-10 text-center font-bold text-lg select-none">1</div>
                <button class="w-12 h-12 rounded-xl flex items-center justify-center text-slate-500 hover:bg-slate-100 hover:text-slate-900 transition-colors text-xl font-medium">+</button>
              </div>
              
              <button @click="addToCart" class="flex-grow flex items-center justify-center gap-3 bg-slate-900 hover:bg-brand-600 text-white py-4 px-8 rounded-2xl font-bold transition-all duration-300 shadow-[0_8px_30px_rgb(245,158,11,0.2)] hover:shadow-[0_8px_40px_rgb(245,158,11,0.4)] text-lg group">
                <svg xmlns="http://www.w3.org/2000/svg" width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="group-hover:scale-110 transition-transform"><path d="M5 12h14"/><path d="M12 5v14"/></svg>
                Add to Cart
              </button>
            </div>
            
            <div class="mt-8 flex items-center gap-3 text-sm text-slate-500 font-medium p-4 bg-green-50/50 rounded-2xl border border-green-100 w-max">
              <span class="w-2.5 h-2.5 rounded-full bg-green-500 animate-pulse relative"><span class="absolute inset-0 bg-green-500 rounded-full animate-ping opacity-75"></span></span>
               In Stock and Ready to Ship Immediately
            </div>
          </div>

        </div>

        <!-- Related Products -->
        <div v-if="relatedProducts.length > 0">
           <h2 class="text-3xl font-extrabold text-slate-900 mb-8 pb-4">You May Also Like</h2>
           <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-6">
              <RouterLink :to="'/product/' + p.id" v-for="p in relatedProducts" :key="p.id" class="bg-white rounded-[2rem] shadow-sm hover:shadow-xl transition-all duration-300 overflow-hidden border border-slate-100 p-5 group flex flex-col items-center text-center">
                <div class="w-full aspect-square rounded-2xl bg-slate-50 overflow-hidden mb-6 relative">
                  <div v-if="p.badge" class="absolute top-4 left-4 bg-brand-500 text-white text-[10px] uppercase tracking-wider font-bold px-3 py-1 rounded-full z-10 shadow-sm">{{ p.badge }}</div>
                  <img :src="p.image" class="w-full h-full object-cover group-hover:scale-105 transition-transform duration-500">
                </div>
                <div class="text-xs text-brand-600 font-bold uppercase tracking-wider mb-2">{{ p.category }}</div>
                <div class="font-extrabold text-slate-900 text-xl mb-3 group-hover:text-brand-600 transition-colors line-clamp-1">{{ p.name }}</div>
                <div class="text-lg font-black text-slate-700">₱{{ p.price.toFixed(2) }}</div>
              </RouterLink>
           </div>
        </div>

      </div>
    </div>
  </div>
</template>
