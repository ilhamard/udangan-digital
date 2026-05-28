<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

// Target date: December 12, 2026, 09:00:00 (WIB / GMT+7)
const weddingDate = new Date('2027-07-12T09:00:00+07:00').getTime()

const days = ref('00')
const hours = ref('00')
const minutes = ref('00')
const seconds = ref('00')
const isExpired = ref(false)

let timerInterval = null

const updateCountdown = () => {
  const now = new Date().getTime()
  const distance = weddingDate - now

  if (distance < 0) {
    isExpired.value = true
    clearInterval(timerInterval)
    return
  }

  const d = Math.floor(distance / (1000 * 60 * 60 * 24))
  const h = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60))
  const m = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60))
  const s = Math.floor((distance % (1000 * 60)) / 1000)

  days.value = d < 10 ? '0' + d : d.toString()
  hours.value = h < 10 ? '0' + h : h.toString()
  minutes.value = m < 10 ? '0' + m : m.toString()
  seconds.value = s < 10 ? '0' + s : s.toString()
}

onMounted(() => {
  updateCountdown()
  timerInterval = setInterval(updateCountdown, 1000)
})

onUnmounted(() => {
  if (timerInterval) clearInterval(timerInterval)
})

// Generate Google Calendar Link
const googleCalendarLink = () => {
  const base = 'https://calendar.google.com/calendar/render?action=TEMPLATE'
  const text = '&text=Pernikahan+Fulan+%26+Fulanah'
  const dates = '&dates=20261212T020000Z/20261212T080000Z' // UTC Time
  const details = '&details=Tanpa+mengurangi+rasa+hormat%2C+kami+mengundang+Anda+untuk+menghadiri+pernikahan+suci+kami.+Semoga+kehadiran+Anda+membawa+keberkahan.'
  const location = '&location=Gedung+Grand+Ballroom+Sakura%2C+Jl.+Kebahagiaan+No.+10%2C+Jakarta'
  return base + text + dates + details + location
}
</script>

<template>
  <section class="section-events">
    <div class="header-container">
      <h3 class="events-sub">WAKTU & TEMPAT ACARA</h3>
      <h2 class="serif-title">Wedding Info</h2>
      <div class="divider-gold">
        <i class="fa-solid fa-heart"></i>
      </div>
    </div>

    <!-- Countdown Timer Card -->
    <div class="glass-card countdown-card animate-fade-in-up">
      <h4 class="countdown-title">Menuju Hari Bahagia</h4>
      <div class="countdown-grid">
        <div class="countdown-item">
          <span class="count-num">{{ days }}</span>
          <span class="count-label">Hari</span>
        </div>
        <div class="countdown-separator">:</div>
        <div class="countdown-item">
          <span class="count-num">{{ hours }}</span>
          <span class="count-label">Jam</span>
        </div>
        <div class="countdown-separator">:</div>
        <div class="countdown-item">
          <span class="count-num">{{ minutes }}</span>
          <span class="count-label">Menit</span>
        </div>
        <div class="countdown-separator">:</div>
        <div class="countdown-item">
          <span class="count-num">{{ seconds }}</span>
          <span class="count-label">Detik</span>
        </div>
      </div>
      <p v-if="isExpired" class="countdown-expired">Acara telah berlangsung!</p>
      
      <a :href="googleCalendarLink()" target="_blank" class="btn-gold btn-calendar">
        <i class="fa-solid fa-calendar-plus"></i> Ingatkan Saya di Google Calendar
      </a>
    </div>

    <!-- Events Details Cards -->
    <div class="events-details">
      <!-- Akad Card -->
      <div class="glass-card event-card animate-fade-in-up">
        <div class="event-icon">
          <i class="fa-solid fa-file-signature"></i>
        </div>
        <h3 class="event-type">Akad Nikah</h3>
        
        <div class="divider-thin"></div>

        <div class="event-info-item">
          <i class="fa-solid fa-calendar-days text-gold"></i>
          <p>Sabtu, 12 Juli 2027</p>
        </div>
        <div class="event-info-item">
          <i class="fa-solid fa-clock text-gold"></i>
          <p>Pukul 09.00 - 11.00 WIB</p>
        </div>
        <div class="event-info-item">
          <i class="fa-solid fa-location-dot text-gold"></i>
          <p>
            <strong>Masjid Raya Al-Hikmah</strong><br>
            Jl. Kebahagiaan No. 8, Kec. Kebon Jeruk, Jakarta Barat
          </p>
        </div>
      </div>

      <!-- Reception Card -->
      <div class="glass-card event-card animate-fade-in-up">
        <div class="event-icon">
          <i class="fa-solid fa-champagne-glasses"></i>
        </div>
        <h3 class="event-type">Resepsi</h3>

        <div class="divider-thin"></div>

        <div class="event-info-item">
          <i class="fa-solid fa-calendar-days text-gold"></i>
          <p>Sabtu, 12 Juli 2027</p>
        </div>
        <div class="event-info-item">
          <i class="fa-solid fa-clock text-gold"></i>
          <p>Pukul 12.00 - 15.00 WIB</p>
        </div>
        <div class="event-info-item">
          <i class="fa-solid fa-location-dot text-gold"></i>
          <p>
            <strong>Gedung Grand Ballroom Sakura</strong><br>
            Jl. Kebahagiaan No. 10, Kec. Kebon Jeruk, Jakarta Barat
          </p>
        </div>
      </div>
    </div>

    <!-- Map Integration Block -->
    <div class="glass-card map-card animate-fade-in-up">
      <h3 class="map-title"><i class="fa-solid fa-map-location-dot text-gold"></i> Peta Lokasi</h3>
      <p class="map-subtitle">Ketuk tombol di bawah untuk melihat rute navigasi Google Maps menuju lokasi acara resepsi.</p>
      
      <!-- Visual Map Mockup -->
      <div class="map-visual-container">
        <div class="map-placeholder-overlay">
          <div class="map-pin">
            <i class="fa-solid fa-location-dot"></i>
            <div class="pin-pulse"></div>
          </div>
          <span class="map-label">Gedung Grand Ballroom Sakura</span>
        </div>
      </div>

      <a 
        href="https://maps.google.com/?q=Gedung+Grand+Ballroom+Sakura+Kebon+Jeruk+Jakarta" 
        target="_blank" 
        class="btn-primary btn-maps"
      >
        <i class="fa-solid fa-route"></i> Petunjuk Rute Google Maps
      </a>
    </div>
  </section>
</template>

<style scoped>
.section-events {
  padding: 3rem 1.5rem;
  background-color: hsl(36, 15%, 94%); /* matching story sand color */
  text-align: center;
}

.header-container {
  margin-bottom: 2.5rem;
}

.events-sub {
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

.countdown-card {
  padding: 1.5rem 1rem;
  border-radius: 24px;
  margin-bottom: 2.5rem;
}

.countdown-title {
  font-family: var(--font-sans);
  font-size: 0.9rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 1px;
  color: var(--color-primary);
  margin-bottom: 1.2rem;
}

.countdown-grid {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  gap: 6px;
  margin-bottom: 1.5rem;
}

.countdown-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  min-width: 60px;
}

.count-num {
  font-family: var(--font-serif);
  font-size: 2rem;
  font-weight: 700;
  color: var(--color-primary-dark);
  line-height: 1;
}

.count-label {
  font-size: 0.7rem;
  color: var(--color-text-muted);
  font-weight: 500;
  margin-top: 4px;
}

.countdown-separator {
  font-size: 1.8rem;
  font-weight: bold;
  color: var(--color-accent);
  line-height: 1;
}

.countdown-expired {
  color: var(--color-danger);
  font-weight: 600;
  margin-bottom: 1rem;
}

.btn-calendar {
  font-size: 0.8rem;
  padding: 0.6rem 1.4rem;
  width: 100%;
  justify-content: center;
  border-radius: 12px;
}

.events-details {
  display: flex;
  flex-direction: column;
  gap: 2rem;
  margin-bottom: 2.5rem;
}

.event-card {
  text-align: left;
  padding: 2rem 1.5rem;
  border-radius: 24px;
  margin-bottom: 0;
}

.event-icon {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  background: rgba(88, 22, 38, 0.08);
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 1.3rem;
  color: var(--color-primary);
  margin-bottom: 1rem;
}

.event-type {
  font-size: 1.4rem;
  color: var(--color-primary);
  margin-bottom: 0.5rem;
}

.divider-thin {
  height: 1px;
  background: linear-gradient(90deg, rgba(212, 175, 55, 0.4), transparent);
  margin-bottom: 1.2rem;
}

.event-info-item {
  display: flex;
  align-items: flex-start;
  gap: 12px;
  margin-bottom: 0.8rem;
  font-size: 0.85rem;
}

.event-info-item:last-child {
  margin-bottom: 0;
}

.event-info-item i {
  font-size: 1rem;
  margin-top: 3px;
}

.event-info-item p {
  color: var(--color-text-dark);
  line-height: 1.5;
}

.event-info-item strong {
  color: var(--color-primary-dark);
}

/* Map Card */
.map-card {
  padding: 1.5rem;
  border-radius: 24px;
  margin-bottom: 0;
}

.map-title {
  font-size: 1.2rem;
  color: var(--color-primary);
  margin-bottom: 0.5rem;
}

.map-subtitle {
  font-size: 0.8rem;
  color: var(--color-text-muted);
  line-height: 1.5;
  margin-bottom: 1.2rem;
}

.map-visual-container {
  height: 160px;
  background: 
    radial-gradient(rgba(255,255,255,0.7), rgba(255,255,255,0.2)),
    url('https://images.unsplash.com/photo-1524661135-423995f22d0b?q=80&w=400&auto=format&fit=crop') center/cover no-repeat;
  border-radius: 16px;
  border: 1px solid rgba(212, 175, 55, 0.2);
  margin-bottom: 1.2rem;
  position: relative;
  overflow: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
}

.map-placeholder-overlay {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
}

.map-pin {
  position: relative;
  font-size: 2.2rem;
  color: var(--color-primary);
  z-index: 2;
}

.pin-pulse {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 40px;
  height: 40px;
  background: rgba(88, 22, 38, 0.2);
  border-radius: 50%;
  z-index: 1;
  animation: pulsePin 1.5s infinite;
}

@keyframes pulsePin {
  0% {
    width: 20px;
    height: 20px;
    opacity: 0.8;
  }
  100% {
    width: 50px;
    height: 50px;
    opacity: 0;
  }
}

.map-label {
  background: var(--color-primary-dark);
  color: #fff;
  font-size: 0.75rem;
  font-weight: 600;
  padding: 0.3rem 0.8rem;
  border-radius: 20px;
  box-shadow: 0 4px 10px rgba(0,0,0,0.2);
}

.btn-maps {
  width: 100%;
  justify-content: center;
  font-size: 0.85rem;
  border-radius: 12px;
}
</style>
