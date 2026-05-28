<script setup>
import { ref, onMounted, nextTick } from 'vue'
import WeddingCover from './components/WeddingCover.vue'
import WeddingProfile from './components/WeddingProfile.vue'
import LoveStory from './components/LoveStory.vue'
import WeddingGallery from './components/WeddingGallery.vue'
import WeddingEvents from './components/WeddingEvents.vue'
import WeddingGift from './components/WeddingGift.vue'
import WeddingRSVP from './components/WeddingRSVP.vue'

const isOpened = ref(false)
const isPlaying = ref(false)
const audioPlayer = ref(null)
const screenContainer = ref(null)

// Falling petals configuration for desktop background decoration
const petals = ref([])

const generatePetals = () => {
  const list = []
  for (let i = 0; i < 20; i++) {
    list.push({
      id: i,
      left: Math.random() * 100 + 'vw',
      delay: Math.random() * 8 + 's',
      duration: Math.random() * 6 + 6 + 's',
      size: Math.random() * 10 + 10 + 'px',
      horizontal: Math.random() * 100 - 50 + 'px'
    })
  }
  petals.value = list
}

onMounted(() => {
  generatePetals()
})

const openInvitation = () => {
  isOpened.value = true
  
  // Unblock screen container scroll
  if (screenContainer.value) {
    screenContainer.value.style.overflowY = 'auto'
  }

  // Play background music
  if (audioPlayer.value) {
    audioPlayer.value.volume = 0.5
    audioPlayer.value.play().then(() => {
      isPlaying.value = true
    }).catch(err => {
      console.warn("Autoplay blocked or audio load error: ", err)
    })
  }
}

const toggleMusic = () => {
  if (!audioPlayer.value) return
  if (isPlaying.value) {
    audioPlayer.value.pause()
    isPlaying.value = false
  } else {
    audioPlayer.value.play()
    isPlaying.value = true
  }
}
</script>

<template>
  <!-- Ambient background for desktop users -->
  <div class="ambient-bg">
    <!-- Falling flower petals -->
    <div class="petals-container">
      <div 
        v-for="p in petals" 
        :key="p.id" 
        class="petal"
        :style="{
          left: p.left,
          animationDelay: p.delay,
          animationDuration: p.duration,
          width: p.size,
          height: p.size,
          '--drift': p.horizontal
        }"
      ></div>
    </div>
  </div>

  <!-- Main Mobile Frame Container -->
  <div class="phone-mockup-wrapper">
    <!-- Status Notch for Phone Frame -->
    <div class="phone-notch"></div>

    <!-- Phone Screen Contents -->
    <div 
      class="phone-screen" 
      ref="screenContainer"
      :style="{ overflowY: isOpened ? 'auto' : 'hidden' }"
    >
      <!-- Cover overlay (Disappear when Buka Undangan clicked) -->
      <WeddingCover v-if="!isOpened" @open="openInvitation" />

      <!-- Main Contents (Only scrolls/fully interacts after cover is open) -->
      <div v-show="isOpened" class="invitation-main-content">
        <!-- Floating audio player -->
        <button 
          class="floating-music-btn" 
          :class="{ 'playing': isPlaying }" 
          @click="toggleMusic"
          aria-label="Toggle Music"
        >
          <i :class="isPlaying ? 'fa-solid fa-music' : 'fa-solid fa-volume-xmark'"></i>
        </button>

        <!-- Components sections layout -->
        <WeddingProfile />
        
        <div class="luxury-spacer">
          <div class="spacer-line"></div>
          <i class="fa-solid fa-heart spacer-heart text-gold"></i>
          <div class="spacer-line"></div>
        </div>

        <LoveStory />

        <div class="luxury-spacer">
          <div class="spacer-line"></div>
          <i class="fa-solid fa-heart spacer-heart text-gold"></i>
          <div class="spacer-line"></div>
        </div>

        <WeddingGallery />

        <div class="luxury-spacer">
          <div class="spacer-line"></div>
          <i class="fa-solid fa-heart spacer-heart text-gold"></i>
          <div class="spacer-line"></div>
        </div>

        <WeddingEvents />

        <div class="luxury-spacer">
          <div class="spacer-line"></div>
          <i class="fa-solid fa-heart spacer-heart text-gold"></i>
          <div class="spacer-line"></div>
        </div>

        <WeddingGift />

        <div class="luxury-spacer">
          <div class="spacer-line"></div>
          <i class="fa-solid fa-heart spacer-heart text-gold"></i>
          <div class="spacer-line"></div>
        </div>

        <WeddingRSVP />

        <!-- Footer credit -->
        <footer class="wedding-footer">
          <h2 class="cursive footer-names">Fulan & Fulanah</h2>
          <p class="footer-thanks">Terima kasih atas doa restu Anda.</p>
          <p class="footer-credit">Made with <i class="fa-solid fa-heart text-gold"></i> for F&F</p>
        </footer>
      </div>
    </div>
  </div>

  <!-- Background Audio Player -->
  <audio 
    ref="audioPlayer" 
    src="/music/once-symponi.mp3"
    loop
  ></audio>
</template>

<style scoped>
.invitation-main-content {
  width: 100%;
  animation: fadeInContent 1.5s ease-out;
}

@keyframes fadeInContent {
  from { opacity: 0; }
  to { opacity: 1; }
}

.luxury-spacer {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 15px;
  padding: 1.5rem 0;
  background-color: var(--color-bg-light);
}

.spacer-line {
  height: 1px;
  width: 50px;
  background: linear-gradient(90deg, transparent, var(--color-accent), transparent);
}

.spacer-heart {
  font-size: 0.75rem;
  opacity: 0.7;
}

.wedding-footer {
  background: var(--color-primary-dark);
  color: #fff;
  padding: 3rem 1.5rem;
  text-align: center;
}

.footer-names {
  font-size: 2.5rem;
  color: var(--color-accent-light);
  margin-bottom: 0.8rem;
}

.footer-thanks {
  font-size: 0.85rem;
  color: var(--color-rose-gold);
  margin-bottom: 1.5rem;
  font-weight: 500;
  letter-spacing: 0.5px;
}

.footer-credit {
  font-size: 0.7rem;
  color: rgba(255,255,255,0.4);
  letter-spacing: 1px;
  text-transform: uppercase;
}

.footer-credit i {
  font-size: 0.65rem;
}
</style>
