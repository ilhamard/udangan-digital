<script setup>
import { ref } from 'vue'

const preweddingPhotos = ref([
  { src: new URL('../assets/prewed-1.jpg', import.meta.url).href, title: 'Menatap Masa Depan' },
  { src: new URL('../assets/prewed-2.jpg', import.meta.url).href, title: 'Kehangatan Kasih' },
  { src: new URL('../assets/prewed-3.jpg', import.meta.url).href, title: 'Langkah Bersama' },
  { src: new URL('../assets/prewed-4.jpg', import.meta.url).href, title: 'Janji Suci' }
])

const activeIndex = ref(null)

const openLightbox = (index) => {
  activeIndex.value = index
}

const closeLightbox = () => {
  activeIndex.value = null
}

const prevPhoto = (e) => {
  e.stopPropagation()
  if (activeIndex.value > 0) {
    activeIndex.value--
  } else {
    activeIndex.value = preweddingPhotos.value.length - 1
  }
}

const nextPhoto = (e) => {
  e.stopPropagation()
  if (activeIndex.value < preweddingPhotos.value.length - 1) {
    activeIndex.value++
  } else {
    activeIndex.value = 0
  }
}
</script>

<template>
  <section class="section-gallery">
    <div class="header-container">
      <h3 class="gallery-sub">GALERI FOTO</h3>
      <h2 class="serif-title">Prewedding Gallery</h2>
      <div class="divider-gold">
        <i class="fa-solid fa-heart"></i>
      </div>
    </div>

    <!-- Gallery Grid -->
    <div class="gallery-grid">
      <div 
        v-for="(photo, index) in preweddingPhotos" 
        :key="index" 
        class="gallery-item animate-fade-in-up"
        @click="openLightbox(index)"
      >
        <div class="gallery-img-container">
          <img :src="photo.src" :alt="photo.title" class="gallery-img" />
          <div class="gallery-overlay">
            <span class="zoom-icon"><i class="fa-solid fa-magnifying-glass-plus"></i></span>
            <p class="photo-title">{{ photo.title }}</p>
          </div>
        </div>
      </div>
    </div>

    <!-- Interactive Lightbox Overlay -->
    <Transition name="fade">
      <div v-if="activeIndex !== null" class="lightbox-overlay" @click="closeLightbox">
        <!-- Close Button -->
        <button class="btn-close" @click="closeLightbox">
          <i class="fa-solid fa-xmark"></i>
        </button>

        <!-- Navigation Buttons -->
        <button class="nav-btn prev-btn" @click="prevPhoto">
          <i class="fa-solid fa-chevron-left"></i>
        </button>
        <button class="nav-btn next-btn" @click="nextPhoto">
          <i class="fa-solid fa-chevron-right"></i>
        </button>

        <!-- Image Display -->
        <div class="lightbox-content" @click.stop>
          <img 
            :src="preweddingPhotos[activeIndex].src" 
            :alt="preweddingPhotos[activeIndex].title" 
            class="lightbox-img" 
          />
          <div class="lightbox-caption">
            <h3>{{ preweddingPhotos[activeIndex].title }}</h3>
            <p>{{ activeIndex + 1 }} dari {{ preweddingPhotos.length }}</p>
          </div>
        </div>
      </div>
    </Transition>
  </section>
</template>

<style scoped>
.section-gallery {
  padding: 3rem 1.5rem;
  background-color: var(--color-bg-light);
  text-align: center;
}

.header-container {
  margin-bottom: 2.5rem;
}

.gallery-sub {
  font-size: 0.75rem;
  letter-spacing: 3px;
  color: var(--color-text-muted);
  margin-bottom: 0.5rem;
  text-transform: uppercase;
}

.serif-title {
  font-size: 2.2rem;
  margin-bottom: 0.2rem;
}

/* Elegant Masonry-like Grid */
.gallery-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 12px;
  max-width: 380px;
  margin: 0 auto;
}

.gallery-item {
  cursor: pointer;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: var(--shadow-sm);
  transition: all 0.3s ease;
  aspect-ratio: 3/4; /* Consistent portrait style prewedding aspect */
}

/* Stagger some items for premium dynamic look */
.gallery-item:nth-child(even) {
  transform: translateY(15px);
}

.gallery-item:hover {
  transform: translateY(-2px) scale(1.02);
  box-shadow: var(--shadow-lg), var(--shadow-gold);
}

.gallery-item:nth-child(even):hover {
  transform: translateY(13px) scale(1.02);
}

.gallery-img-container {
  position: relative;
  width: 100%;
  height: 100%;
  background-color: var(--color-rose-gold);
}

.gallery-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
  transition: transform 0.5s ease;
}

.gallery-item:hover .gallery-img {
  transform: scale(1.08);
}

.gallery-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(to top, rgba(29, 4, 9, 0.8) 0%, rgba(29, 4, 9, 0.1) 60%);
  opacity: 0;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  align-items: center;
  padding: 1rem;
  transition: opacity 0.3s ease;
  z-index: 2;
}

.gallery-item:hover .gallery-overlay {
  opacity: 1;
}

.zoom-icon {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 1.5rem;
  color: #fff;
  background: rgba(212, 175, 55, 0.8);
  width: 44px;
  height: 44px;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  box-shadow: 0 0 10px rgba(0,0,0,0.3);
}

.photo-title {
  color: #fff;
  font-size: 0.75rem;
  font-weight: 500;
  letter-spacing: 1px;
  text-shadow: 0 1px 2px rgba(0,0,0,0.5);
  margin-top: auto;
}

/* Lightbox Overlay Styling */
.lightbox-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background: rgba(18, 2, 7, 0.95);
  z-index: 9999;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: zoom-out;
}

.lightbox-content {
  position: relative;
  max-width: 90%;
  max-height: 80%;
  display: flex;
  flex-direction: column;
  align-items: center;
  animation: zoomIn 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.15) forwards;
}

@keyframes zoomIn {
  from {
    transform: scale(0.9);
    opacity: 0;
  }
  to {
    transform: scale(1);
    opacity: 1;
  }
}

.lightbox-img {
  max-width: 100%;
  max-height: 70vh;
  border: 3px solid var(--color-accent);
  border-radius: 12px;
  object-fit: contain;
  box-shadow: 0 20px 40px rgba(0,0,0,0.5);
}

.lightbox-caption {
  color: #fff;
  text-align: center;
  margin-top: 1rem;
}

.lightbox-caption h3 {
  font-family: var(--font-serif);
  font-size: 1.2rem;
  color: var(--color-accent-light);
  margin-bottom: 0.2rem;
}

.lightbox-caption p {
  font-size: 0.75rem;
  color: rgba(255,255,255,0.5);
}

/* Navigation buttons */
.btn-close {
  position: absolute;
  top: 24px;
  right: 24px;
  background: rgba(255, 255, 255, 0.15);
  border: none;
  color: #fff;
  width: 44px;
  height: 44px;
  border-radius: 50%;
  font-size: 1.3rem;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  transition: all 0.3s ease;
  z-index: 10005;
}

.btn-close:hover {
  background: var(--color-primary);
  color: #fff;
  transform: rotate(90deg);
}

.nav-btn {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(255, 255, 255, 0.1);
  border: none;
  color: #fff;
  width: 48px;
  height: 48px;
  border-radius: 50%;
  font-size: 1.1rem;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  transition: all 0.3s ease;
  z-index: 10002;
}

.nav-btn:hover {
  background: var(--color-accent);
  color: #fff;
}

.prev-btn {
  left: 20px;
}

.next-btn {
  right: 20px;
}

/* Hide navigation buttons on narrow screens for simple tap gestures */
@media (max-width: 480px) {
  .nav-btn {
    width: 38px;
    height: 38px;
    font-size: 0.9rem;
  }
  .prev-btn { left: 10px; }
  .next-btn { right: 10px; }
}

/* Vue Transition styling */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
