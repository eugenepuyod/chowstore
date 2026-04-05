<script setup>
import { ref } from 'vue'
import { onMounted } from 'vue'

onMounted(() => {
  window.scrollTo({ top: 0, behavior: 'smooth' })
})

const formStatus = ref(null)

const submitContactForm = () => {
  formStatus.value = 'Message sent successfully! We will get back to you shortly.'
  setTimeout(() => { formStatus.value = null }, 4000)
}
</script>

<template>
  <div class="bg-white min-h-screen">
    <!-- Header -->
    
    <section class="bg-brand-50 py-24 sm:py-32 relative overflow-hidden">
      <div class="absolute inset-0 bg-gradient-to-br from-brand-100 to-white opacity-80 z-0"></div>
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 relative z-10 text-center">
        <h1 class="text-4xl md:text-6xl font-extrabold text-slate-900 tracking-tight mb-6">Contact Us</h1>
        <p class="text-xl text-slate-600 max-w-3xl mx-auto leading-relaxed">
          Have questions about our Chow-approved products or an existing order? Send us a message.
        </p>
      </div>
    </section>

    <div class="py-16 sm:py-24">
      <!-- Contact Form -->
      <div class="bg-white rounded-3xl p-8 md:p-12 shadow-[0_8px_30px_rgb(0,0,0,0.04)] border border-slate-100 mb-16 relative overflow-hidden">
        <!-- decorative bg -->
        <div class="absolute right-0 top-0 -mr-20 -mt-20 w-64 h-64 bg-gradient-to-bl from-brand-100 to-transparent rounded-full opacity-60"></div>
        
        <form @submit.prevent="submitContactForm" class="relative z-10 max-w-2xl mx-auto space-y-6">
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <div>
              <label for="name" class="block text-sm font-semibold text-slate-700 mb-2">Full Name</label>
              <input type="text" id="name" required class="w-full px-5 py-4 rounded-2xl border border-slate-200 bg-slate-50 focus:bg-white focus:outline-none focus:ring-2 focus:ring-brand-500 transition-all shadow-sm" placeholder="Eugene Doe">
            </div>
            <div>
              <label for="email" class="block text-sm font-semibold text-slate-700 mb-2">Email Address</label>
              <input type="email" id="email" required class="w-full px-5 py-4 rounded-2xl border border-slate-200 bg-slate-50 focus:bg-white focus:outline-none focus:ring-2 focus:ring-brand-500 transition-all shadow-sm" placeholder="eugene@chowstore.com">
            </div>
          </div>
          <div>
            <label for="subject" class="block text-sm font-semibold text-slate-700 mb-2">Subject</label>
            <input type="text" id="subject" class="w-full px-5 py-4 rounded-2xl border border-slate-200 bg-slate-50 focus:bg-white focus:outline-none focus:ring-2 focus:ring-brand-500 transition-all shadow-sm" placeholder="How can we help?">
          </div>
          <div>
            <label for="message" class="block text-sm font-semibold text-slate-700 mb-2">Message</label>
            <textarea id="message" rows="6" required class="w-full px-5 py-4 rounded-2xl border border-slate-200 bg-slate-50 focus:bg-white focus:outline-none focus:ring-2 focus:ring-brand-500 transition-all shadow-sm resize-none" placeholder="Write your message here..."></textarea>
          </div>
          
          <button type="submit" class="w-full py-5 bg-slate-900 text-white font-bold rounded-2xl hover:bg-brand-600 transition-all duration-300 shadow-xl shadow-slate-900/20 hover:shadow-brand-500/30">
            Send Message
          </button>
          
          <transition name="fade">
            <div v-if="formStatus" class="mt-6 p-5 bg-green-50 border border-green-200 text-green-700 rounded-2xl text-center font-medium">
              {{ formStatus }}
            </div>
          </transition>
        </form>
      </div>

      <!-- Google Map -->
      <div class="text-center mb-6">
        <h3 class="text-2xl font-bold text-slate-900 mb-2">Visit Our Headquarters</h3>
        <p class="text-slate-500">Drop by if you are in the neighborhood.</p>
      </div>
      <div class="w-full h-96 rounded-3xl overflow-hidden shadow-lg border border-slate-200">
        <iframe 
          src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3924.5701348460893!2d123.97450567354606!3d10.376214266451257!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x33a9bd4cf481250b%3A0x871b5901b0beac61!2sAnami%20Homes%20North!5e0!3m2!1sen!2sph!4v1775173242885!5m2!1sen!2sph" 
          width="100%" 
          height="100%" 
          style="border:0;" 
          allowfullscreen="" 
          loading="lazy" 
          referrerpolicy="no-referrer-when-downgrade">
        </iframe>
      </div>
    </div>
  </div>
</template>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease, transform 0.3s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateY(10px);
}
</style>
