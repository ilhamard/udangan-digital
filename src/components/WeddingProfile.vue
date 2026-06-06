<template>
  <section class="section-profile">
    <!-- Decorative Lace Top -->
    <div class="lace-divider">
      <i class="fa-solid fa-leaf text-gold"></i>
      <i class="fa-solid fa-seedling text-gold"></i>
      <i class="fa-solid fa-leaf text-gold"></i>
    </div>

    <!-- Islamic Verse -->
    <div class="glass-card verse-card animate-fade-in-up">
      <i class="fa-solid fa-quote-left quote-icon"></i>
      <p class="arabic-text">وَمِنْ آيَاتِهِ أَنْ خَلَقَ لَكُم مِّنْ أَنفُسِكُمْ أَزْوَاجًا لِّتَسْكُنُوا إِلَيْهَا وَجَعَلَ بَيْنَكُم مَّوَدَّةً وَرَحْمَةً ۚ إِنَّ فِي ذَٰلِكَ لَآيَاتٍ لِّقَوْمٍ يَتَفَكَّرُونَ</p>
      <p class="verse-translation">
        "Dan di antara tanda-tanda (kebesaran)-Nya ialah Dia menciptakan pasangan-pasangan untukmu dari jenismu sendiri, agar kamu cenderung dan merasa tenteram kepadanya, dan Dia menjadikan di antaramu rasa kasih dan sayang. Sungguh, pada yang demikian itu benar-benar terdapat tanda-tanda (kebesaran-Nya) bagi kaum yang berpikir."
      </p>
      <p class="verse-reference">- QS. Ar-Rum: 21 -</p>
    </div>

    <!-- Opening Greeting -->
    <div class="greeting-block">
      <h2 class="serif-title">Assalamu'alaikum Wr. Wb.</h2>
      <p class="greeting-text">
        Dengan memohon rahmat dan ridho Allah SWT, kami dengan penuh rasa syukur bermaksud mengundang Bapak/Ibu/Saudara/i untuk menghadiri perayaan pernikahan kami:
      </p>
    </div>

    <!-- The Groom & Bride Cards -->
    <div class="couple-profiles">
      <!-- Groom Card -->
      <div class="profile-card animate-fade-in-up">
        <div class="avatar-wrapper">
          <div class="gold-ring"></div>
          <img src="../assets/ilham.png" alt="Fulan" class="avatar-img" />
        </div>
        <h2 class="cursive groom-name">Ilham Ardiansyah</h2>
        <p class="parent-info">
          Putra Kedua dari <br>
          <strong>Bapak H. Ahmad Fauzi</strong> <br>
          & <strong>Ibu Hj. Siti Aminah</strong>
        </p>
        <div class="social-links">
          <a href="https://www.instagram.com/ilhamardians_/" target="_blank" class="instagram-link">
            <i class="fa-brands fa-instagram"></i> @ilhamardians_
          </a>
        </div>
      </div>

      <!-- And separator -->
      <div class="ampersand-divider">
        <span class="cursive">&</span>
      </div>

      <!-- Bride Card -->
      <div class="profile-card animate-fade-in-up">
        <div class="avatar-wrapper">
          <div class="gold-ring"></div>
          <img src="../assets/ana.png" alt="Fulanah" class="avatar-img" />
        </div>
        <h2 class="cursive bride-name">Siti Rokhana</h2>
        <p class="parent-info">
          Putri Kedua dari <br>
          <strong>Bapak Drs. H. Hermawan</strong> <br>
          & <strong>Ibu Hj. Rahmawati</strong>
        </p>
        <div class="social-links">
          <a href="https://www.instagram.com/rsiti_/" target="_blank" class="instagram-link">
            <i class="fa-brands fa-instagram"></i> @rsiti_
          </a>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.section-profile {
  padding: 3rem 1.5rem;
  background-color: var(--color-bg-light);
  text-align: center;
}

.lace-divider {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 15px;
  margin-bottom: 2rem;
}

.lace-divider i {
  font-size: 1.2rem;
  opacity: 0.7;
}

.verse-card {
  margin-bottom: 2.5rem;
  padding: 2rem 1.5rem;
  border-radius: 24px;
}

.quote-icon {
  font-size: 1.8rem;
  color: var(--color-rose-gold);
  margin-bottom: 1rem;
}

.arabic-text {
  font-size: 1.1rem;
  line-height: 2.2;
  color: var(--color-primary-dark);
  font-weight: 600;
  margin-bottom: 1rem;
  font-family: 'Times New Roman', serif; /* Classic arabic look */
}

.verse-translation {
  font-size: 0.8rem;
  color: var(--color-text-muted);
  font-style: italic;
  line-height: 1.6;
  margin-bottom: 0.8rem;
}

.verse-reference {
  font-size: 0.8rem;
  font-weight: 600;
  color: var(--color-accent);
  letter-spacing: 1px;
}

.greeting-block {
  margin-bottom: 3rem;
}

.serif-title {
  font-size: 1.8rem;
  margin-bottom: 1rem;
}

.greeting-text {
  font-size: 0.85rem;
  color: var(--color-text-muted);
  line-height: 1.7;
  max-width: 320px;
  margin: 0 auto;
}

.couple-profiles {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2rem;
}

.profile-card {
  width: 100%;
  max-width: 280px;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 1rem;
}

.avatar-wrapper {
  position: relative;
  width: 150px;
  height: 150px;
  margin-bottom: 1.5rem;
}

.gold-ring {
  position: absolute;
  top: -5px;
  left: -5px;
  right: -5px;
  bottom: -5px;
  border: 2px dashed var(--color-accent);
  border-radius: 50%;
  animation: rotateRing 20s linear infinite;
}

@keyframes rotateRing {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}

.avatar-img {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  object-fit: cover;
  border: 4px solid #fff;
  box-shadow: var(--shadow-md);
  background-color: var(--color-rose-gold);
}

.groom-name, .bride-name {
  font-size: 2.2rem;
  color: var(--color-primary);
  margin-bottom: 0.5rem;
}

.parent-info {
  font-size: 0.85rem;
  line-height: 1.6;
  color: var(--color-text-muted);
  margin-bottom: 1rem;
}

.parent-info strong {
  color: var(--color-primary-dark);
}

.ampersand-divider {
  font-size: 3rem;
  color: var(--color-accent);
  line-height: 1;
  margin: 0.5rem 0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.social-links {
  margin-top: 0.3rem;
}

.instagram-link {
  font-size: 0.8rem;
  color: var(--color-primary);
  text-decoration: none;
  font-weight: 600;
  border: 1px solid rgba(88, 22, 38, 0.2);
  padding: 0.4rem 1rem;
  border-radius: 20px;
  display: flex;
  align-items: center;
  gap: 5px;
  background-color: rgba(255,255,255,0.6);
  transition: all 0.3s ease;
}

.instagram-link:hover {
  background-color: var(--color-primary);
  color: #fff;
  border-color: var(--color-primary);
}
</style>
