<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  guestName: {
    type: String,
    default: ''
  }
})

const emit = defineEmits(['open'])

const isAnimating = ref(false)

const displayGuestName = computed(() => {
  return props.guestName || 'Tamu Undangan'
})

const handleOpen = () => {
  isAnimating.value = true
  // Let the slide up animation complete before emitting 'open'
  setTimeout(() => {
    emit('open')
  }, 1000) // matches transition duration
}
</script>

<template>
  <div class="cover-wrapper" :class="{ 'slide-up': isAnimating }">
    <div class="cover-bg-overlay"></div>
    
    <div class="cover-content">
      <!-- Calligraphy Header -->
      <div class="monogram-container animate-fade-in">
        <span class="monogram cursive">F & F</span>
      </div>

      <p class="invite-sub animate-fade-in-delayed">WALIMATUL 'URSY</p>
      
      <h1 class="couple-names cursive animate-fade-in-delayed-2">
        Ilham & Ana
      </h1>

      <div class="divider-gold animate-fade-in-delayed-2">
        <i class="fa-solid fa-heart"></i>
      </div>

      <!-- Guest Card -->
      <div class="guest-card animate-fade-in-delayed-3">
        <p class="guest-label">Kepada Yth. Bapak/Ibu/Saudara/i</p>
        <h3 class="guest-name">{{ displayGuestName }}</h3>
        <p class="guest-note">Kami Mengundang Anda Untuk Hadir di Pernikahan Kami</p>
      </div>

      <!-- Call to Action -->
      <div class="action-container animate-fade-in-delayed-4">
        <button class="btn-gold btn-open" @click="handleOpen">
          <i class="fa-solid fa-envelope-open-text"></i>
          Buka Undangan
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.cover-wrapper {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1000;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
  background-color: #120207;
  transition: transform 1.2s cubic-bezier(0.77, 0, 0.175, 1);
}

.cover-wrapper.slide-up {
  transform: translateY(-100%);
}

.cover-bg-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  /* Beautiful luxury dark burgundy radial gradient mixed with watercolor effect */
  background: 
    linear-gradient(to bottom, rgba(41, 5, 13, 0.85), rgba(18, 2, 7, 0.95)),
    url('../assets/cover-bg.jpg') center/cover no-repeat;
  filter: saturate(1.1);
  z-index: 1;
}

.cover-content {
  position: relative;
  z-index: 2;
  width: 90%;
  max-width: 360px;
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem 1.5rem;
}

.monogram-container {
  width: 90px;
  height: 90px;
  border-radius: 50%;
  border: 2px solid var(--color-accent);
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 1.5rem;
  box-shadow: var(--shadow-gold), inset 0 0 10px rgba(212, 175, 55, 0.2);
  background: rgba(212, 175, 55, 0.05);
}

.monogram {
  font-size: 2.2rem;
  color: var(--color-accent);
  line-height: 1;
  text-shadow: 0 2px 4px rgba(0,0,0,0.3);
}

.invite-sub {
  font-family: var(--font-sans);
  font-size: 0.75rem;
  letter-spacing: 4px;
  color: var(--color-rose-gold);
  margin-bottom: 0.5rem;
  font-weight: 500;
}

.couple-names {
  font-size: 3.2rem;
  color: #fff;
  line-height: 1.2;
  margin-bottom: 0.5rem;
  text-shadow: 0 2px 10px rgba(0, 0, 0, 0.4);
}

.guest-card {
  background: rgba(255, 255, 255, 0.07);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.15);
  border-radius: 20px;
  padding: 1.5rem 1.2rem;
  margin: 2rem 0;
  width: 100%;
  box-shadow: 0 15px 35px rgba(0,0,0,0.3);
}

.guest-label {
  font-size: 0.75rem;
  color: var(--color-rose-gold);
  text-transform: uppercase;
  letter-spacing: 1px;
  margin-bottom: 0.5rem;
}

.guest-name {
  font-family: var(--font-serif);
  font-size: 1.4rem;
  color: var(--color-accent-light);
  font-weight: 600;
  margin-bottom: 0.5rem;
  text-shadow: 0 1px 2px rgba(0,0,0,0.5);
}

.guest-note {
  font-size: 0.75rem;
  color: rgba(255, 255, 255, 0.6);
  line-height: 1.4;
}

.btn-open {
  padding: 0.9rem 2.2rem;
  font-size: 0.95rem;
  animation: pulseGold 2s infinite;
}

@keyframes pulseGold {
  0% {
    box-shadow: 0 0 0 0 rgba(212, 175, 55, 0.6);
  }
  70% {
    box-shadow: 0 0 0 12px rgba(212, 175, 55, 0);
  }
  100% {
    box-shadow: 0 0 0 0 rgba(212, 175, 55, 0);
  }
}

/* Base Animations */
.animate-fade-in {
  opacity: 0;
  transform: scale(0.9);
  animation: fadeInScale 1s forwards cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.animate-fade-in-delayed {
  opacity: 0;
  transform: translateY(10px);
  animation: fadeInUp 0.8s 0.3s forwards ease-out;
}

.animate-fade-in-delayed-2 {
  opacity: 0;
  transform: translateY(15px);
  animation: fadeInUp 0.8s 0.6s forwards ease-out;
}

.animate-fade-in-delayed-3 {
  opacity: 0;
  transform: translateY(20px);
  animation: fadeInUp 0.8s 0.9s forwards ease-out;
}

.animate-fade-in-delayed-4 {
  opacity: 0;
  transform: translateY(20px);
  animation: fadeInUp 0.8s 1.2s forwards ease-out;
}

@keyframes fadeInScale {
  to {
    opacity: 1;
    transform: scale(1);
  }
}

@keyframes fadeInUp {
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>
