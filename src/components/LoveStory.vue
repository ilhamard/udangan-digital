<template>
  <section class="section-story">
    <div class="header-container">
      <h3 class="story-sub">BAGAIMANA KAMI BERTEMU</h3>
      <h2 class="serif-title">Love Story</h2>
      <div class="divider-gold">
        <i class="fa-solid fa-heart"></i>
      </div>
    </div>

    <!-- Timeline Container -->
    <div class="timeline-container">
      <!-- Story Node 1 -->
      <div class="timeline-item animate-fade-in-up">
        <div class="timeline-badge">
          <i class="fa-solid fa-book-open"></i>
        </div>
        <div class="glass-card timeline-card">
          <div class="card-date text-gold">Maret 2024</div>
          <h4 class="card-title">Awal Bertemu (Perkenalan)</h4>
          <p class="card-desc">
            Pertama kali kami diperkenalkan oleh seorang sahabat baik di sebuah perpustakaan umum di kota. Dari obrolan santai mengenai buku favorit, kami menyadari memiliki banyak pandangan hidup yang selaras.
          </p>
        </div>
      </div>

      <!-- Story Node 2 -->
      <div class="timeline-item animate-fade-in-up">
        <div class="timeline-badge">
          <i class="fa-solid fa-comments"></i>
        </div>
        <div class="glass-card timeline-card">
          <div class="card-date text-gold">Oktober 2025</div>
          <h4 class="card-title">Masa Penjajakan</h4>
          <p class="card-desc">
            Setelah sekian lama berkomunikasi dan saling mengenal visi misi hidup masing-masing, kami mantap untuk melangkah ke hubungan yang lebih terarah dan diberkahi. Kami yakin satu sama lain adalah pelengkap iman yang dicari.
          </p>
        </div>
      </div>

      <!-- Story Node 3 -->
      <div class="timeline-item animate-fade-in-up">
        <div class="timeline-badge">
          <i class="fa-solid fa-ring"></i>
        </div>
        <div class="glass-card timeline-card">
          <div class="card-date text-gold">Januari 2026</div>
          <h4 class="card-title">Hari Khitbah (Lamaran)</h4>
          <p class="card-desc">
            Dengan restu tulus dari kedua belah pihak orang tua dan keluarga besar, Fulan secara resmi melamar Fulanah. Komitmen suci dibentuk untuk mempersiapkan bahtera rumah tangga yang sakinah, mawaddah, dan warahmah.
          </p>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.section-story {
  padding: 3rem 1.5rem;
  background-color: hsl(36, 15%, 94%); /* Slightly darker warm sand for background transition */
  text-align: center;
  position: relative;
}

.header-container {
  margin-bottom: 3rem;
}

.story-sub {
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

/* Timeline vertical layout */
.timeline-container {
  position: relative;
  max-width: 380px;
  margin: 0 auto;
  padding-left: 2.5rem;
  text-align: left;
}

/* The vertical line in timeline */
.timeline-container::before {
  content: '';
  position: absolute;
  top: 10px;
  left: 17px;
  width: 2px;
  height: calc(100% - 20px);
  background: linear-gradient(to bottom, var(--color-rose-gold) 0%, var(--color-accent) 50%, var(--color-rose-gold) 100%);
}

.timeline-item {
  position: relative;
  margin-bottom: 2.5rem;
}

.timeline-item:last-child {
  margin-bottom: 0;
}

/* Badge (Circle icon on timeline) */
.timeline-badge {
  position: absolute;
  left: -33px;
  top: 5px;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  background: var(--color-primary);
  border: 2px solid var(--color-accent);
  color: #fff;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 0.95rem;
  z-index: 5;
  box-shadow: var(--shadow-sm), var(--shadow-gold);
}

.timeline-card {
  margin-bottom: 0; /* Override generic glass card bottom margin */
  padding: 1.5rem 1.2rem;
  transition: all 0.3s ease;
}

.timeline-card:hover {
  transform: translateX(3px);
  box-shadow: var(--shadow-lg);
  border-color: var(--color-accent);
}

.card-date {
  font-size: 0.8rem;
  font-weight: 700;
  letter-spacing: 1px;
  margin-bottom: 0.4rem;
}

.card-title {
  font-family: var(--font-sans);
  font-size: 1.05rem;
  font-weight: 700;
  color: var(--color-primary);
  margin-bottom: 0.6rem;
}

.card-desc {
  font-size: 0.8rem;
  line-height: 1.6;
  color: var(--color-text-dark);
}
</style>
