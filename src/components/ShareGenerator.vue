<script setup>
import { ref, computed, onMounted } from 'vue'

const guestNameInput = ref('')
const selectedTone = ref('formal') // 'formal' or 'casual'
const isPanelOpen = ref(false)
const showToast = ref(false)
const toastMessage = ref('')

const togglePanel = () => {
  isPanelOpen.value = !isPanelOpen.value
}

// Compute the share link based on current domain + guest name query
const generatedLink = computed(() => {
  const base = window.location.origin + window.location.pathname
  if (!guestNameInput.value.trim()) {
    return base
  }
  const formattedName = encodeURIComponent(guestNameInput.value.trim())
  return `${base}?to=${formattedName}`
})

// Custom templates for WhatsApp sharing
const shareMessage = computed(() => {
  const name = guestNameInput.value.trim() || '[Nama Tamu]'
  const link = generatedLink.value

  if (selectedTone.value === 'formal') {
    return `Kepada Yth.
*Bapak/Ibu/Saudara/i: ${name}*

Tanpa mengurangi rasa hormat, perkenankan kami mengundang Bapak/Ibu/Saudara/i untuk menghadiri acara pernikahan kami (Ilham & Ana).

Detail undangan digital kami dapat diakses melalui link berikut:
${link}

Merupakan suatu kehormatan dan kebahagiaan bagi kami apabila Bapak/Ibu/Saudara/i berkenan hadir dan memberikan doa restu.

Terima kasih.
_Ilham & Ana_`
  } else {
    return `Halo *${name}*,

Di hari yang spesial ini, kami ingin mengundang kamu untuk hadir di acara pernikahan kami (Ilham & Ana).

Detail lengkap acaranya bisa kamu lihat di link undangan digital berikut ya:
${link}

Kehadiran dan doa restumu sangat berarti bagi kami. Sampai jumpa di hari bahagia kami!

Terima kasih.
_Ilham & Ana_`
  }
})

// Trigger a brief toast message
const triggerToast = (msg) => {
  toastMessage.value = msg
  showToast.value = true
  setTimeout(() => {
    showToast.value = false
  }, 2500)
}

// Copy the message to clipboard
const copyToClipboard = async () => {
  try {
    await navigator.clipboard.writeText(shareMessage.value)
    triggerToast('Pesan & Link berhasil disalin!')
  } catch (err) {
    console.error('Gagal menyalin teks: ', err)
    triggerToast('Gagal menyalin, silakan salin manual.')
  }
}

// Copy only the URL to clipboard
const copyLinkOnly = async () => {
  try {
    await navigator.clipboard.writeText(generatedLink.value)
    triggerToast('Link undangan berhasil disalin!')
  } catch (err) {
    console.error('Gagal menyalin link: ', err)
  }
}

// Share directly to WhatsApp Web / Mobile App
const shareToWhatsApp = () => {
  const text = encodeURIComponent(shareMessage.value)
  const whatsappUrl = `https://api.whatsapp.com/send?text=${text}`
  window.open(whatsappUrl, '_blank')
}
</script>

<template>
  <div class="admin-panel-wrapper">
    <!-- Floating Admin Trigger Button -->
    <button 
      class="floating-admin-btn" 
      @click="togglePanel"
      :class="{ 'active': isPanelOpen }"
      title="Buka Generator Link Undangan"
      aria-label="Generator Undangan"
    >
      <i class="fa-solid" :class="isPanelOpen ? 'fa-xmark' : 'fa-share-nodes'"></i>
      <span v-if="!isPanelOpen" class="badge-admin">Admin Tool</span>
    </button>

    <!-- Slide Up Glassmorphic Drawer -->
    <Transition name="slide-panel">
      <div v-if="isPanelOpen" class="generator-drawer glass-card">
        <div class="drawer-header">
          <h3><i class="fa-solid fa-wand-magic-sparkles text-gold"></i> Pengirim Undangan</h3>
          <span class="admin-subtitle">Buat link khusus WhatsApp tamu</span>
        </div>

        <div class="drawer-body">
          <!-- Input Guest Name -->
          <div class="form-group">
            <label class="form-label" for="admin-guest-name">Nama Tamu Undangan</label>
            <input 
              id="admin-guest-name"
              type="text" 
              class="form-control" 
              v-model="guestNameInput"
              placeholder="Contoh: Budi Santoso & Istri" 
              autofocus
            />
          </div>

          <!-- Select Tone -->
          <div class="form-group">
            <label class="form-label">Gaya Bahasa (Template)</label>
            <div class="tone-toggle">
              <button 
                class="tone-btn" 
                :class="{ 'active': selectedTone === 'formal' }"
                @click="selectedTone = 'formal'"
              >
                <i class="fa-solid fa-building"></i> Resmi / Formal
              </button>
              <button 
                class="tone-btn" 
                :class="{ 'active': selectedTone === 'casual' }"
                @click="selectedTone = 'casual'"
              >
                <i class="fa-solid fa-mug-hot"></i> Santai / Teman
              </button>
            </div>
          </div>

          <!-- Live Preview -->
          <div class="preview-box">
            <div class="preview-header">
              <span>Pratinjau Pesan WhatsApp:</span>
            </div>
            <pre class="preview-content">{{ shareMessage }}</pre>
          </div>

          <!-- Action Buttons -->
          <div class="action-grid">
            <button class="btn-primary flex-btn" @click="copyToClipboard">
              <i class="fa-solid fa-copy"></i> Salin Pesan
            </button>
            
            <button class="btn-gold flex-btn" @click="shareToWhatsApp">
              <i class="fa-brands fa-whatsapp"></i> Kirim WhatsApp
            </button>
          </div>

          <button class="btn-link-only" @click="copyLinkOnly">
            <i class="fa-solid fa-link"></i> Hanya Salin Link Undangan
          </button>
        </div>
      </div>
    </Transition>

    <!-- Toast Alert Notification -->
    <div class="toast-notification" :class="{ 'show': showToast }">
      <i class="fa-solid fa-circle-check text-gold"></i>
      <span>{{ toastMessage }}</span>
    </div>
  </div>
</template>

<style scoped>
.admin-panel-wrapper {
  position: fixed;
  bottom: 0;
  right: 0;
  width: 100vw;
  display: flex;
  justify-content: center;
  pointer-events: none;
  z-index: 2000;
}

/* Floating admin activator button */
.floating-admin-btn {
  position: fixed;
  bottom: 24px;
  right: 24px;
  width: 56px;
  height: 56px;
  border-radius: 50%;
  background: linear-gradient(135deg, #1b030b 0%, #3a0a16 100%);
  border: 2px solid var(--color-accent);
  color: #fff;
  font-size: 1.3rem;
  cursor: pointer;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.5), var(--shadow-gold);
  display: flex;
  justify-content: center;
  align-items: center;
  pointer-events: auto;
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.floating-admin-btn:hover {
  transform: scale(1.08) rotate(15deg);
}

.floating-admin-btn.active {
  background: var(--color-primary);
  border-color: #fff;
  transform: rotate(90deg);
}

.badge-admin {
  position: absolute;
  top: -10px;
  right: -5px;
  background: var(--color-accent);
  color: var(--color-primary-dark);
  font-size: 0.65rem;
  font-weight: 700;
  padding: 0.15rem 0.4rem;
  border-radius: 10px;
  box-shadow: 0 2px 5px rgba(0,0,0,0.3);
  white-space: nowrap;
}

/* Sliding Drawer Card styling */
.generator-drawer {
  position: fixed;
  bottom: 96px;
  right: 24px;
  width: 90vw;
  max-width: 380px;
  max-height: 80vh;
  overflow-y: auto;
  pointer-events: auto;
  margin-bottom: 0;
  border: 1px solid rgba(212, 175, 55, 0.4);
  box-shadow: 0 15px 40px rgba(0, 0, 0, 0.4);
  z-index: 1999;
}

.drawer-header {
  border-bottom: 1px solid rgba(212, 175, 55, 0.15);
  padding-bottom: 0.8rem;
  margin-bottom: 1.2rem;
}

.drawer-header h3 {
  font-size: 1.15rem;
  display: flex;
  align-items: center;
  gap: 8px;
}

.admin-subtitle {
  font-size: 0.7rem;
  color: var(--color-text-muted);
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.drawer-body {
  display: flex;
  flex-direction: column;
  gap: 0.8rem;
}

/* Tone Selection Styles */
.tone-toggle {
  display: flex;
  gap: 8px;
  width: 100%;
}

.tone-btn {
  flex: 1;
  background: rgba(255, 255, 255, 0.6);
  border: 1px solid rgba(212, 175, 55, 0.2);
  border-radius: 10px;
  padding: 0.6rem;
  font-size: 0.75rem;
  font-family: var(--font-sans);
  font-weight: 600;
  color: var(--color-text-muted);
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  transition: all 0.3s ease;
}

.tone-btn.active {
  background: var(--color-primary);
  border-color: var(--color-accent);
  color: #fff;
  box-shadow: var(--shadow-sm);
}

/* Message text preview block */
.preview-box {
  background: rgba(18, 2, 7, 0.04);
  border: 1px solid rgba(212, 175, 55, 0.15);
  border-radius: 12px;
  padding: 0.8rem;
  max-height: 160px;
  overflow-y: auto;
}

.preview-header {
  font-size: 0.7rem;
  color: var(--color-text-muted);
  margin-bottom: 0.4rem;
  font-weight: 600;
  text-transform: uppercase;
}

.preview-content {
  font-family: monospace;
  font-size: 0.75rem;
  color: var(--color-text-dark);
  white-space: pre-wrap;
  line-height: 1.4;
  text-align: left;
}

/* Actions layout */
.action-grid {
  display: flex;
  gap: 8px;
  margin-top: 0.5rem;
}

.flex-btn {
  flex: 1;
  justify-content: center;
  font-size: 0.85rem;
  padding: 0.75rem 1rem;
  border-radius: 12px;
}

.btn-link-only {
  background: transparent;
  border: none;
  color: var(--color-primary);
  font-family: var(--font-sans);
  font-size: 0.75rem;
  font-weight: 600;
  cursor: pointer;
  text-align: center;
  margin-top: 0.4rem;
  padding: 0.4rem;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 5px;
  transition: opacity 0.2s;
}

.btn-link-only:hover {
  opacity: 0.8;
  text-decoration: underline;
}

/* Animations for panel sliding */
.slide-panel-enter-active,
.slide-panel-leave-active {
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.15);
}

.slide-panel-enter-from {
  opacity: 0;
  transform: translateY(40px) scale(0.95);
}

.slide-panel-leave-to {
  opacity: 0;
  transform: translateY(40px) scale(0.95);
}

/* Toast container override */
.toast-notification {
  pointer-events: none;
}
</style>
