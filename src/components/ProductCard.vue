<script setup>
import { RouterLink } from 'vue-router'

const props = defineProps({
  product: {
    type: Object,
    required: true
  },
  isList: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits(['add-to-cart'])
</script>

<template>
  <div :class="[
    'bg-white rounded-2xl shadow-[0_4px_20px_-10px_rgba(0,0,0,0.1)] hover:shadow-xl transition-all duration-300 overflow-hidden border border-slate-100 group relative',
    isList ? 'flex flex-col sm:flex-row min-h-[160px]' : 'flex flex-col h-full hover:-translate-y-1'
  ]">
    
    <RouterLink :to="'/product/' + product.id" :class="isList ? 'w-full sm:w-48 flex-shrink-0 relative overflow-hidden block' : 'relative aspect-square w-full overflow-hidden block'">
      <img :src="product.image" :alt="product.name" class="w-full h-full object-cover group-hover:scale-105 transition-transform duration-500">
      
      <!-- Overlay badges -->
      <div v-if="product.badge" class="absolute top-4 left-4 bg-rose-500 text-white text-xs font-bold px-3 py-1 rounded-full shadow-sm z-10">
        {{ product.badge }}
      </div>
      
      <!-- Hover Overlay -->
      <div class="absolute inset-0 bg-slate-900/10 opacity-0 group-hover:opacity-100 transition-opacity z-0"></div>
    </RouterLink>
    
    <div :class="['p-5 flex flex-col bg-white', isList ? 'flex-grow justify-center' : 'flex-grow']">
      <div class="flex justify-between items-start mb-2">
        <div>
          <div class="text-xs text-brand-600 font-semibold tracking-wider uppercase mb-1">{{ product.category }}</div>
          <RouterLink :to="'/product/' + product.id" class="text-lg font-bold text-slate-900 group-hover:text-brand-600 transition-colors line-clamp-1 block">
            {{ product.name }}
          </RouterLink>
        </div>
        <div class="text-lg font-black text-slate-800 ml-2">₱{{ product.price.toFixed(2) }}</div>
      </div>
      
      <p :class="['text-slate-500 text-sm mb-4 leading-relaxed', isList ? 'line-clamp-2' : 'line-clamp-2 mt-2']">{{ product.description }}</p>
      
      <!-- Tags Mini Display -->
      <div v-if="product.tags" class="flex gap-1 flex-wrap mb-4">
         <span v-for="tag in product.tags.slice(0,2)" :key="tag" class="px-2 py-0.5 bg-slate-100 text-slate-500 text-[10px] uppercase tracking-wider font-bold rounded-md">
           {{ tag }}
         </span>
         <span v-if="product.tags.length > 2" class="px-2 py-0.5 bg-slate-100 text-slate-500 text-[10px] uppercase tracking-wider font-bold rounded-md">...</span>
      </div>
      
      <div :class="['mt-auto', isList ? 'flex justify-end' : '']">
        <button @click.stop="$emit('add-to-cart', product)" :class="[
          'flex items-center justify-center gap-2 bg-slate-100 hover:bg-slate-900 text-slate-700 hover:text-white py-2.5 rounded-xl font-bold transition-colors',
          isList ? 'w-full sm:w-auto sm:px-6' : 'w-full'
        ]">
          <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M5 12h14"/><path d="M12 5v14"/></svg>
          Add to Cart
        </button>
      </div>
    </div>
  </div>
</template>
