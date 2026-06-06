<script setup>
import { ref, onMounted } from 'vue'

const props = defineProps({
  guestName: {
    type: String,
    default: ''
  }
})

const nameInput = ref('')
const attendanceInput = ref('datang') // 'datang' or 'tidak_datang'
const wishInput = ref('')

const wishes = ref([])

// Sample seed data to populate the guest board initially
const seedWishes = [
  {
    name: 'Siti Rahma & Keluarga',
    attendance: 'datang',
    message: 'Selamat untuk Fulan dan Fulanah! Semoga pernikahan ini menjadi awal perjalanan yang penuh keberkahan, cinta, dan kebahagiaan yang langgeng. Aamiin YRA.',
    timestamp: '2 jam yang lalu'
  },
  {
    name: 'Budi Santoso (Sahabat Fulan)',
    attendance: 'datang',
    message: 'Selamat menempuh hidup baru bro! Akhirnya berlabuh juga di pelaminan. Semoga dilancarkan semua acaranya dan menjadi keluarga sakinah mawaddah warahmah!',
    timestamp: '5 jam yang lalu'
  },
  {
    name: 'Dewi Lestari',
    attendance: 'tidak_datang',
    message: 'Selamat berbahagia Fulanah dan suami. Mohon maaf belum bisa hadir secara langsung karena sedang bertugas di luar kota. Doa terbaik kami kirimkan dari jauh. Selamat!',
    timestamp: '1 hari yang lalu'
  }
]

onMounted(() => {
  if (props.guestName) {
    nameInput.value = props.guestName
  }

  const stored = localStorage.getItem('wedding_wishes')
  if (stored) {
    try {
      wishes.value = JSON.parse(stored)
    } catch (e) {
      wishes.value = [...seedWishes]
    }
  } else {
    wishes.value = [...seedWishes]
    localStorage.setItem('wedding_wishes', JSON.stringify(seedWishes))
  }
})

const handleSubmit = () => {
  if (!nameInput.value.trim() || !wishInput.value.trim()) return

  const newWish = {
    name: nameInput.value.trim(),
    attendance: attendanceInput.value,
    message: wishInput.value.trim(),
    timestamp: 'Baru saja'
  }

  // Prepend to wishes list
  wishes.value = [newWish, ...wishes.value]
  
  // Save to local storage
  localStorage.setItem('wedding_wishes', JSON.stringify(wishes.value))

  // Reset inputs
  nameInput.value = ''
  wishInput.value = ''
  attendanceInput.value = 'datang'
}
</script>

<template>
  <section class="section-rsvp">
    <div class="header-container">
      <h3 class="rsvp-sub">BUKU TAMU & RSVP</h3>
      <h2 class="serif-title">Ucapan & Kehadiran</h2>
      <div class="divider-gold">
        <i class="fa-solid fa-heart"></i>
      </div>
      <p class="rsvp-intro">
        Merupakan suatu kehormatan dan kebahagiaan bagi kami apabila Bapak/Ibu/Saudara/i berkenan hadir dan memberikan doa restu bagi kami.
      </p>
    </div>

    <!-- RSVP & Wishes Form -->
    <div class="glass-card rsvp-form-card animate-fade-in-up">
      <h3 class="form-title"><i class="fa-solid fa-pen-to-square text-gold"></i> Kirim Doa & Ucapan</h3>
      
      <form @submit.prevent="handleSubmit" class="rsvp-form">
        <!-- Name -->
        <div class="form-group">
          <label for="guest-name-input" class="form-label">Nama Anda</label>
          <input 
            type="text" 
            id="guest-name-input" 
            class="form-control" 
            v-model="nameInput" 
            placeholder="Masukkan Nama Lengkap" 
            required 
          />
        </div>

        <!-- Attendance Status -->
        <div class="form-group">
          <label class="form-label">Konfirmasi Kehadiran</label>
          <div class="attendance-options">
            <label class="attendance-btn" :class="{ 'active': attendanceInput === 'datang' }">
              <input 
                type="radio" 
                name="attendance" 
                value="datang" 
                v-model="attendanceInput" 
                class="hidden-radio"
              />
              <i class="fa-solid fa-circle-check"></i> Datang
            </label>
            
            <label class="attendance-btn non-attend" :class="{ 'active': attendanceInput === 'tidak_datang' }">
              <input 
                type="radio" 
                name="attendance" 
                value="tidak_datang" 
                v-model="attendanceInput" 
                class="hidden-radio"
              />
              <i class="fa-solid fa-circle-xmark"></i> Tidak Datang
            </label>
          </div>
        </div>

        <!-- Wish Message -->
        <div class="form-group">
          <label for="wish-message-input" class="form-label">Doa & Ucapan Restu</label>
          <textarea 
            id="wish-message-input" 
            class="form-control wish-textarea" 
            v-model="wishInput" 
            placeholder="Tuliskan ucapan selamat dan doa restu Anda..." 
            rows="4" 
            required
          ></textarea>
        </div>

        <!-- Submit Button -->
        <button type="submit" class="btn-primary btn-submit">
          <i class="fa-solid fa-paper-plane"></i> Kirim Ucapan
        </button>
      </form>
    </div>

    <!-- Wishes Board -->
    <div class="wishes-board animate-fade-in-up">
      <h3 class="board-title">
        <i class="fa-solid fa-comments text-gold"></i> Doa Restu Kerabat 
        <span class="wishes-count">({{ wishes.length }})</span>
      </h3>

      <div class="wishes-scroll-box">
        <TransitionGroup name="list" tag="div" class="wishes-list">
          <div 
            v-for="(wish, index) in wishes" 
            :key="wish.name + index" 
            class="wish-item glass-card"
          >
            <div class="wish-header">
              <h4 class="wish-sender">{{ wish.name }}</h4>
              <span 
                class="attendance-badge" 
                :class="wish.attendance === 'datang' ? 'badge-attend' : 'badge-absent'"
              >
                <i :class="wish.attendance === 'datang' ? 'fa-solid fa-circle-check' : 'fa-solid fa-circle-xmark'"></i>
                {{ wish.attendance === 'datang' ? 'Hadir' : 'Tidak Hadir' }}
              </span>
            </div>
            
            <p class="wish-msg">{{ wish.message }}</p>
            
            <div class="wish-footer">
              <span class="wish-time"><i class="fa-regular fa-clock"></i> {{ wish.timestamp }}</span>
            </div>
          </div>
        </TransitionGroup>
      </div>
    </div>
  </section>
</template>

<style scoped>
.section-rsvp {
  padding: 3rem 1.5rem;
  background-color: hsl(36, 15%, 94%); /* matching timeline */
  text-align: center;
}

.header-container {
  margin-bottom: 2.5rem;
}

.rsvp-sub {
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

.rsvp-intro {
  font-size: 0.85rem;
  color: var(--color-text-muted);
  line-height: 1.6;
  max-width: 320px;
  margin: 1rem auto 0 auto;
}

/* Form Styles */
.rsvp-form-card {
  padding: 1.8rem 1.2rem;
  border-radius: 24px;
  margin-bottom: 2.5rem;
}

.form-title {
  font-family: var(--font-sans);
  font-size: 1.1rem;
  font-weight: 700;
  color: var(--color-primary);
  margin-bottom: 1.5rem;
}

.attendance-options {
  display: flex;
  gap: 10px;
}

.attendance-btn {
  flex: 1;
  border: 1px solid rgba(212, 175, 55, 0.3);
  padding: 0.65rem 0.5rem;
  border-radius: 10px;
  font-size: 0.8rem;
  font-weight: 600;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  cursor: pointer;
  background: rgba(255,255,255,0.7);
  color: var(--color-text-muted);
  transition: all 0.3s ease;
}

.attendance-btn i {
  font-size: 0.9rem;
}

.attendance-btn.active {
  border-color: var(--color-success);
  background: hsla(150, 60%, 35%, 0.1);
  color: var(--color-success);
  box-shadow: 0 0 10px rgba(76, 175, 80, 0.1);
}

.attendance-btn.non-attend.active {
  border-color: var(--color-danger);
  background: hsla(350, 70%, 45%, 0.1);
  color: var(--color-danger);
  box-shadow: 0 0 10px rgba(244, 67, 54, 0.1);
}

.hidden-radio {
  display: none;
}

.wish-textarea {
  resize: none;
}

.btn-submit {
  width: 100%;
  justify-content: center;
  font-size: 0.9rem;
  padding: 0.8rem 1.8rem;
  border-radius: 12px;
  margin-top: 0.5rem;
}

/* Board Styles */
.wishes-board {
  text-align: left;
}

.board-title {
  font-family: var(--font-sans);
  font-size: 1.1rem;
  font-weight: 700;
  color: var(--color-primary);
  margin-bottom: 1.2rem;
  display: flex;
  align-items: center;
  gap: 8px;
}

.wishes-count {
  font-size: 0.85rem;
  color: var(--color-text-muted);
  font-weight: normal;
}

.wishes-scroll-box {
  max-height: 480px;
  overflow-y: auto;
  padding-right: 4px;
}

/* Scrollbar styling for board */
.wishes-scroll-box::-webkit-scrollbar {
  width: 4px;
}
.wishes-scroll-box::-webkit-scrollbar-track {
  background: transparent;
}
.wishes-scroll-box::-webkit-scrollbar-thumb {
  background: var(--color-rose-gold);
  border-radius: 4px;
}

.wishes-list {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.wish-item {
  padding: 1.2rem;
  border-radius: 18px;
  margin-bottom: 0; /* Override */
}

.wish-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 8px;
  margin-bottom: 0.6rem;
}

.wish-sender {
  font-family: var(--font-sans);
  font-size: 0.9rem;
  font-weight: 700;
  color: var(--color-primary-dark);
}

.attendance-badge {
  font-size: 0.7rem;
  font-weight: 600;
  padding: 0.2rem 0.6rem;
  border-radius: 12px;
  display: flex;
  align-items: center;
  gap: 4px;
  white-space: nowrap;
}

.badge-attend {
  background-color: hsla(150, 60%, 35%, 0.1);
  color: var(--color-success);
  border: 1px solid rgba(76, 175, 80, 0.2);
}

.badge-absent {
  background-color: hsla(350, 70%, 45%, 0.1);
  color: var(--color-danger);
  border: 1px solid rgba(244, 67, 54, 0.2);
}

.wish-msg {
  font-size: 0.8rem;
  line-height: 1.6;
  color: var(--color-text-dark);
  margin-bottom: 0.6rem;
  white-space: pre-line;
}

.wish-footer {
  display: flex;
  justify-content: flex-end;
}

.wish-time {
  font-size: 0.7rem;
  color: var(--color-text-muted);
}

/* Transition Group Animations for live board updates */
.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}
.list-enter-from {
  opacity: 0;
  transform: translateY(-20px);
}
.list-leave-to {
  opacity: 0;
  transform: translateY(20px);
}
</style>
