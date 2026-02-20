<script setup lang="js">
  useHead({
    link: [
      { rel: 'icon', type: 'image/x-icon', href: '/img/motor.png' }
    ]
  })
</script>

<template>
  <div class="screen-fit bg-gradient">
    <!-- Header -->
    <div class="header-gradient px-8 py-3 text-white shadow-xl">
      <div class="header-content">
        <img src="/img/motor.png" alt="JD Motorräder Logo" class="logo" />
        <h1>JD Motorräder</h1>
      </div>
    </div>

    <!-- Main Content -->
    <div class="content-area container mx-auto px-6">
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 max-w-7xl mx-auto">
        
        <!-- Box 1: Fahrzeugverwaltung -->
        <div class="card card-blue">
          <div class="icon-circle icon-blue">
            <svg class="w-10 h-10" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"/>
            </svg>
          </div>
          <h2>Fahrzeugverwaltung</h2>
          <p class="card-description">Motorradbestand</p>
        </div>

        <!-- Box 2: Kundenverwaltung -->
        <div class="card card-gray">
          <div class="icon-circle icon-gray">
            <svg class="w-10 h-10" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0zm6 3a2 2 0 11-4 0 2 2 0 014 0zM7 10a2 2 0 11-4 0 2 2 0 014 0z"/>
            </svg>
          </div>
          <h2>Kundenverwaltung</h2>
          <p class="card-description">Kundendaten</p>
        </div>

        <!-- Box 3: Teillagerverwaltung -->
        <div class="card card-blue">
          <div class="icon-circle icon-blue">
            <svg class="w-10 h-10" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20 7l-8-4-8 4m16 0l-8 4m8-4v10l-8 4m0-10L4 7m8 4v10M4 7v10l8 4"/>
            </svg>
          </div>
          <h2>Teillagerverwaltung</h2>
          <p class="card-description">Ersatzteile und Lagerbestände</p>
        </div>

        <!-- Box 4: Werkstattverwaltung -->
        <div class="card card-gray">
          <div class="icon-circle icon-gray">
            <svg class="w-10 h-10" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10.325 4.317c.426-1.756 2.924-1.756 3.35 0a1.724 1.724 0 002.573 1.066c1.543-.94 3.31.826 2.37 2.37a1.724 1.724 0 001.065 2.572c1.756.426 1.756 2.924 0 3.35a1.724 1.724 0 00-1.066 2.573c.94 1.543-.826 3.31-2.37 2.37a1.724 1.724 0 00-2.572 1.065c-.426 1.756-2.924 1.756-3.35 0a1.724 1.724 0 00-2.573-1.066c-1.543.94-3.31-.826-2.37-2.37a1.724 1.724 0 00-1.065-2.572c-1.756-.426-1.756-2.924 0-3.35a1.724 1.724 0 001.066-2.573c-.94-1.543.826-3.31 2.37-2.37.996.608 2.296.07 2.572-1.065z"/>
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z"/>
            </svg>
          </div>
          <h2>Werkstattverwaltung</h2>
          <p class="card-description">Reparaturen und Wartungen</p>
        </div>

        <!-- Box 5: Terminkalender -->
        <div class="card card-blue">
          <div class="icon-circle icon-blue">
            <svg class="w-10 h-10" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"/>
            </svg>
          </div>
          <h2>Terminkalender</h2>
          <p class="card-description">Termine</p>
        </div>

        <!-- Box 6: Persönlicher Bereich -->
        <div class="card card-gray">
          <div class="icon-circle icon-gray">
            <svg class="w-10 h-10" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"/>
            </svg>
          </div>
          <h2>Persönlicher Bereich</h2>
          <p class="card-description">Profile und Einstellungen</p>
        </div>

      </div>
    </div>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap');

.screen-fit {
  height: 100vh;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

.bg-gradient {
  background: linear-gradient(135deg, #e0e7ff 0%, #dbeafe 50%, #f0f9ff 100%);
}

.header-gradient {
  background: linear-gradient(135deg, #475569 0%, #2563eb 50%, #475569 100%);
  position: relative;
  flex-shrink: 0;
}

.header-content {
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  max-width: 1400px;
  margin: 0 auto;
}

.logo {
  position: absolute;
  left: 0;
  height: 55px;
  width: auto;
  object-fit: contain;
  filter: drop-shadow(0 4px 8px rgba(0, 0, 0, 0.2));
}

h1 {
  font-family: 'Inter', sans-serif;
  font-size: 2.25rem;
  text-align: center;
  font-weight: 700;
  letter-spacing: 0.05em;
}

h2 {
  font-family: 'Inter', sans-serif;
  font-size: 1.3rem;
  text-align: center;
  font-weight: 600;
  margin-bottom: 0.25rem;
  letter-spacing: 0.02em;
}

.content-area {
  flex: 1;
  display: flex;
  align-items: center;
  padding-top: 1rem;
  padding-bottom: 1rem;
}

.card {
  border-radius: 1.25rem;
  padding: 1.75rem 1.25rem;
  text-align: center;
  color: white;
  cursor: pointer;
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.1);
}

.card-blue {
  background: linear-gradient(135deg, #3b82f6 0%, #2563eb 100%);
}

.card-blue:hover {
  background: linear-gradient(135deg, #2563eb 0%, #1d4ed8 100%);
  box-shadow: 0 12px 32px rgba(37, 99, 235, 0.4);
  transform: translateY(-6px);
}

.card-gray {
  background: linear-gradient(135deg, #64748b 0%, #475569 100%);
}

.card-gray:hover {
  background: linear-gradient(135deg, #475569 0%, #334155 100%);
  box-shadow: 0 12px 32px rgba(71, 85, 105, 0.4);
  transform: translateY(-6px);
}

.icon-circle {
  width: 65px;
  height: 65px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto 1rem;
  transition: all 0.3s ease;
  background: rgba(255, 255, 255, 0.2);
  border: 2px solid rgba(255, 255, 255, 0.3);
}

.card:hover .icon-circle {
  transform: scale(1.1);
  background: rgba(255, 255, 255, 0.3);
  box-shadow: 0 0 20px rgba(255, 255, 255, 0.3);
}

.card-description {
  font-size: 0.95rem;
  opacity: 0.95;
  margin-top: 0.25rem;
  line-height: 1.4;
}

.container {
  max-width: 1400px;
  width: 100%;
}

@media (max-width: 768px) {
  .screen-fit {
    height: auto;
    overflow: auto;
  }

  h1 {
    font-size: 1.75rem;
  }

  .logo {
    height: 45px;
  }
}
</style>