<script setup>
import { ref } from 'vue'

const accounts = ref([
  { bankName: 'BCA', accountNumber: '1234567890', owner: 'Fulan' },
  { bankName: 'MANDIRI', accountNumber: '0987654321', owner: 'Fulanah' }
])

const physicalAddress = 'Jl. Kebahagiaan No. 10, Kec. Kebon Jeruk, Jakarta Barat (Gedung Grand Ballroom Sakura)'

const toastMessage = ref('')
const isToastShown = ref(false)

const triggerToast = (message) => {
  toastMessage.value = message
  isToastShown.value = true
  setTimeout(() => {
    isToastShown.value = false
  }, 2500)
}

const copyText = async (text, label) => {
  try {
    await navigator.clipboard.writeText(text)
    triggerToast(`${label} berhasil disalin!`)
  } catch (err) {
    // Fallback if navigator.clipboard is unavailable
    const textArea = document.createElement('textarea')
    textArea.value = text
    document.body.appendChild(textArea)
    textArea.select()
    try {
      document.execCommand('copy')
      triggerToast(`${label} berhasil disalin!`)
    } catch (fallbackErr) {
      triggerToast('Gagal menyalin. Silakan salin manual.')
    }
    document.body.removeChild(textArea)
  }
}
</script>

<template>
  <section class="section-gift">
    <!-- Local Toast Notification -->
    <div class="toast-notification" :class="{ 'show': isToastShown }">
      <i class="fa-solid fa-circle-check text-gold"></i>
      <span>{{ toastMessage }}</span>
    </div>

    <div class="header-container">
      <h3 class="gift-sub">KADO DIGITAL</h3>
      <h2 class="serif-title">Wedding Gift</h2>
      <div class="divider-gold">
        <i class="fa-solid fa-heart"></i>
      </div>
      <p class="gift-intro">
        Doa restu Anda merupakan karunia terindah bagi kami. Namun jika Anda ingin memberikan tanda kasih secara digital maupun fisik, Anda dapat menyalurkannya melalui pilihan di bawah ini:
      </p>
    </div>

    <!-- Digital Bank Accounts -->
    <div class="bank-accounts">
      <div 
        v-for="(acc, index) in accounts" 
        :key="index" 
        class="glass-card bank-card animate-fade-in-up"
      >
        <div class="bank-header">
          <span class="bank-badge">{{ acc.bankName }}</span>
          <i class="fa-solid fa-credit-card bank-icon"></i>
        </div>
        
        <div class="bank-details">
          <p class="account-label">Nomor Rekening:</p>
          <h3 class="account-number">{{ acc.accountNumber }}</h3>
          <p class="account-owner">a.n. {{ acc.owner }}</p>
        </div>

        <button 
          class="btn-gold btn-copy" 
          @click="copyText(acc.accountNumber, 'Nomor rekening')"
        >
          <i class="fa-solid fa-copy"></i> Salin Rekening
        </button>
      </div>
    </div>

    <!-- Physical Gift Address -->
    <div class="glass-card address-card animate-fade-in-up">
      <div class="address-header">
        <i class="fa-solid fa-gift gift-box-icon text-gold"></i>
        <h3>Kirim Kado Fisik</h3>
      </div>
      
      <div class="divider-thin"></div>
      
      <p class="address-text">{{ physicalAddress }}</p>
      
      <button 
        class="btn-primary btn-copy-address" 
        @click="copyText(physicalAddress, 'Alamat pengiriman')"
      >
        <i class="fa-solid fa-map-pin"></i> Salin Alamat Pengiriman
      </button>
    </div>
  </section>
</template>

<style scoped>
.section-gift {
  padding: 3rem 1.5rem;
  background-color: var(--color-bg-light);
  text-align: center;
  position: relative;
}

.header-container {
  margin-bottom: 2.5rem;
}

.gift-sub {
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

.gift-intro {
  font-size: 0.85rem;
  color: var(--color-text-muted);
  line-height: 1.6;
  max-width: 320px;
  margin: 1rem auto 0 auto;
}

.bank-accounts {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  margin-bottom: 2.5rem;
}

.bank-card {
  text-align: left;
  padding: 1.8rem 1.5rem;
  border-radius: 24px;
  margin-bottom: 0;
}

.bank-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1.2rem;
}

.bank-badge {
  background: var(--color-primary);
  color: #fff;
  font-size: 0.85rem;
  font-weight: 700;
  padding: 0.3rem 0.9rem;
  border-radius: 6px;
  letter-spacing: 1px;
  box-shadow: var(--shadow-sm);
}

.bank-icon {
  font-size: 1.4rem;
  color: var(--color-rose-gold);
}

.bank-details {
  margin-bottom: 1.2rem;
}

.account-label {
  font-size: 0.75rem;
  color: var(--color-text-muted);
  margin-bottom: 0.2rem;
}

.account-number {
  font-family: var(--font-serif);
  font-size: 1.5rem;
  color: var(--color-primary-dark);
  font-weight: 700;
  letter-spacing: 1px;
  margin-bottom: 0.2rem;
}

.account-owner {
  font-size: 0.85rem;
  font-weight: 600;
  color: var(--color-text-dark);
}

.btn-copy {
  width: 100%;
  justify-content: center;
  font-size: 0.8rem;
  border-radius: 12px;
}

/* Address Card */
.address-card {
  padding: 2rem 1.5rem;
  border-radius: 24px;
  margin-bottom: 0;
  text-align: left;
}

.address-header {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 0.8rem;
}

.address-header h3 {
  font-family: var(--font-sans);
  font-size: 1.1rem;
  font-weight: 700;
  color: var(--color-primary);
}

.gift-box-icon {
  font-size: 1.3rem;
}

.divider-thin {
  height: 1px;
  background: linear-gradient(90deg, rgba(212, 175, 55, 0.4), transparent);
  margin-bottom: 1rem;
}

.address-text {
  font-size: 0.85rem;
  color: var(--color-text-dark);
  line-height: 1.6;
  margin-bottom: 1.5rem;
}

.btn-copy-address {
  width: 100%;
  justify-content: center;
  font-size: 0.8rem;
  border-radius: 12px;
}
</style>
