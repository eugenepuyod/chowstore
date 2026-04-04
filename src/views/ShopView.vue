<script setup>
import { ref, computed } from 'vue'
import ProductCard from '../components/ProductCard.vue'
import { products, categoriesList, tagsList } from '../data/products'
import { onMounted } from 'vue'

onMounted(() => {
  window.scrollTo({ top: 0, behavior: 'smooth' })
})

const isListView = ref(false)
const currentPage = ref(1)
const itemsPerPage = 6

const searchQueryInput = ref('')
const activeSearchQuery = ref('')
const executeSearch = () => {
  activeSearchQuery.value = searchQueryInput.value
  updateFilters()
}
const selectedCategory = ref('All')
const maxPrice = ref(50000)
const selectedTags = ref([])

const toggleTag = (tag) => {
  if (selectedTags.value.includes(tag)) {
    selectedTags.value = selectedTags.value.filter(t => t !== tag)
  } else {
    selectedTags.value.push(tag)
  }
  currentPage.value = 1 // Reset to first page
}

const filteredProducts = computed(() => {
  return products.filter(p => {
    // 1. Category Match
    if (selectedCategory.value !== 'All' && p.category !== selectedCategory.value) return false
    // 2. Price Match
    if (p.price > maxPrice.value) return false
    // 3. Search Match
    if (activeSearchQuery.value && !p.name.toLowerCase().includes(activeSearchQuery.value.toLowerCase())) return false
    // 4. Tags Match (if tags selected, product must have at least one)
    if (selectedTags.value.length > 0) {
      if (!p.tags) return false
      const hasMatchingTag = p.tags.some(tag => selectedTags.value.includes(tag))
      if (!hasMatchingTag) return false
    }
    return true
  })
})

const totalPages = computed(() => {
  return Math.ceil(filteredProducts.value.length / itemsPerPage) || 1
})

const paginatedProducts = computed(() => {
  const start = (currentPage.value - 1) * itemsPerPage
  const end = start + itemsPerPage
  return filteredProducts.value.slice(start, end)
})

const nextPage = () => {
  if (currentPage.value < totalPages.value) currentPage.value++
}
const prevPage = () => {
  if (currentPage.value > 1) currentPage.value--
}
const setPage = (p) => {
  currentPage.value = p
}

const addToCart = (product) => {
  alert(`Added ${product.name} to cart! (Mock)`)
}

// Watchers could reset page to 1 on filter changes but simple reactive reset is done on clicks.
const updateFilters = () => { currentPage.value = 1 }
</script>

<template>
  <div class="bg-slate-50 min-h-screen py-16">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 w-full">
      
      <!-- Top Title and Controls -->
      <div class="flex flex-col md:flex-row justify-between items-end mb-10 pb-6 border-b border-slate-200">
        <div>
          <h1 class="text-4xl font-extrabold text-slate-900 mb-2 tracking-tight">Shop Collection</h1>
          <p class="text-lg text-slate-600 max-w-2xl">Discover quality dogs, nutritious food, essential vitamins, and stylish accessories.</p>
        </div>
        
        <div class="mt-6 md:mt-0 flex gap-2">
          <button @click="isListView = false" :class="[
            'p-2 rounded-lg transition-colors border',
            !isListView ? 'bg-white border-brand-500 text-brand-600 shadow-[0_4px_10px_-4px_rgba(245,158,11,0.5)]' : 'bg-transparent border-transparent text-slate-400 hover:text-slate-600 hover:bg-slate-200'
          ]">
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect width="7" height="7" x="3" y="3" rx="1"/><rect width="7" height="7" x="14" y="3" rx="1"/><rect width="7" height="7" x="14" y="14" rx="1"/><rect width="7" height="7" x="3" y="14" rx="1"/></svg>
          </button>
          <button @click="isListView = true" :class="[
            'p-2 rounded-lg transition-colors border',
            isListView ? 'bg-white border-brand-500 text-brand-600 shadow-[0_4px_10px_-4px_rgba(245,158,11,0.5)]' : 'bg-transparent border-transparent text-slate-400 hover:text-slate-600 hover:bg-slate-200'
          ]">
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="8" x2="21" y1="6" y2="6"/><line x1="8" x2="21" y1="12" y2="12"/><line x1="8" x2="21" y1="18" y2="18"/><line x1="3" x2="3.01" y1="6" y2="6"/><line x1="3" x2="3.01" y1="12" y2="12"/><line x1="3" x2="3.01" y1="18" y2="18"/></svg>
          </button>
        </div>
      </div>

      <div class="flex flex-col lg:flex-row gap-10">
        
        <!-- Sidebar Filters -->
        <aside class="w-full lg:w-1/4 flex flex-col gap-8">
          
          <!-- Search -->
          <div>
            <div class="relative">
              <input v-model="searchQueryInput" @keyup.enter="executeSearch" type="text" placeholder="Type and hit Enter to search..." class="w-full pl-10 pr-4 py-3 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-brand-500 transition-shadow">
              <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="absolute left-3 top-1/2 -translate-y-1/2 text-slate-400"><circle cx="11" cy="11" r="8"/><path d="m21 21-4.3-4.3"/></svg>
            </div>
          </div>

          <!-- Categories -->
          <div class="bg-white p-6 rounded-2xl border border-slate-100 shadow-sm">
             <h3 class="font-bold text-slate-800 mb-4 tracking-wide uppercase text-sm">Categories</h3>
             <div class="flex flex-col gap-2">
               <button v-for="cat in categoriesList" :key="cat" @click="selectedCategory = cat; updateFilters()" :class="[
                 'text-left px-3 py-2 rounded-xl font-medium transition-colors text-sm',
                 selectedCategory === cat ? 'bg-brand-50 text-brand-700 shadow-sm' : 'text-slate-600 hover:bg-slate-50 hover:text-slate-900'
               ]">
                 {{ cat }}
               </button>
             </div>
          </div>

          <!-- Price Range -->
          <div class="bg-white p-6 rounded-2xl border border-slate-100 shadow-sm">
             <h3 class="font-bold text-slate-800 mb-4 tracking-wide uppercase text-sm">Max Price: <span class="text-brand-600">₱{{ maxPrice }}</span></h3>
             <input type="range" v-model="maxPrice" @input="updateFilters" min="10" max="50000" step="10" class="w-full accent-brand-500 cursor-pointer">
             <div class="flex justify-between text-xs text-slate-400 mt-2 font-medium">
               <span>₱10</span>
               <span>₱50000</span>
             </div>
          </div>

          <!-- Tags -->
          <div class="bg-white p-6 rounded-2xl border border-slate-100 shadow-sm">
             <h3 class="font-bold text-slate-800 mb-4 tracking-wide uppercase text-sm">Tags</h3>
             <div class="flex flex-wrap gap-2">
               <button v-for="tag in tagsList" :key="tag" @click="toggleTag(tag)" :class="[
                 'px-3 py-1.5 rounded-lg border text-xs font-semibold tracking-wider uppercase transition-colors',
                 selectedTags.includes(tag) ? 'bg-brand-500 border-brand-500 text-white shadow-sm hover:bg-brand-600' : 'bg-white border-slate-200 text-slate-500 hover:border-slate-300 hover:bg-slate-50'
               ]">
                 {{ tag }}
               </button>
             </div>
          </div>

        </aside>

        <!-- Main Product Grid/List -->
        <div class="w-full lg:w-3/4 flex flex-col">
          
          <div v-if="filteredProducts.length === 0" class="flex-grow flex flex-col items-center justify-center p-12 bg-white rounded-[2rem] border border-slate-100 shadow-[0_4px_20px_-10px_rgba(0,0,0,0.05)] border-dashed">
            <div class="w-20 h-20 bg-slate-50 rounded-full flex items-center justify-center mb-4">
               <span class="text-4xl">🔍</span>
            </div>
            <h3 class="text-2xl font-bold text-slate-800 mb-2">No products found</h3>
            <p class="text-slate-500 text-center max-w-sm">Try adjusting your filters, category, or search term to quickly find what you're looking for.</p>
            <button @click="searchQueryInput=''; activeSearchQuery=''; selectedCategory='All'; maxPrice=50000; selectedTags=[]; updateFilters()" class="mt-6 px-8 py-3 bg-slate-900 text-white font-bold rounded-xl hover:bg-brand-600 transition-colors shadow-m">Clear All Filters</button>
          </div>
          
          <div v-else :class="isListView ? 'flex flex-col gap-6' : 'grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6'">
            <ProductCard 
              v-for="product in paginatedProducts" 
              :key="product.id" 
              :product="product" 
              :isList="isListView"
              @add-to-cart="addToCart" 
            />
          </div>
          
          <!-- Pagination -->
          <div v-if="totalPages > 1" class="mt-16 flex items-center justify-center gap-3">
            <button @click="prevPage" :disabled="currentPage === 1" class="w-12 h-12 flex items-center justify-center rounded-xl border border-slate-200 bg-white text-slate-600 disabled:opacity-30 disabled:cursor-not-allowed hover:bg-slate-50 transition-colors shadow-sm">
              <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m15 18-6-6 6-6"/></svg>
            </button>
            
            <button v-for="p in totalPages" :key="p" @click="setPage(p)" :class="[
              'w-12 h-12 flex items-center justify-center rounded-xl font-bold text-lg transition-colors shadow-sm',
              currentPage === p ? 'bg-brand-600 text-white border border-transparent' : 'bg-white border border-slate-200 text-slate-600 hover:bg-slate-50'
            ]">
              {{ p }}
            </button>
            
            <button @click="nextPage" :disabled="currentPage === totalPages" class="w-12 h-12 flex items-center justify-center rounded-xl border border-slate-200 bg-white text-slate-600 disabled:opacity-30 disabled:cursor-not-allowed hover:bg-slate-50 transition-colors shadow-sm">
              <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m9 18 6-6-6-6"/></svg>
            </button>
          </div>
          
        </div>
      </div>
      
    </div>
  </div>
</template>
