<script>
import './style.css'

export default {
  name: 'App',
  data() {
    return {
      cats: [
        {
          name: 'Pick',
          image: '/imagen/Pick.jpg',
          clicks: 0,
        },
        {
          name: 'PACO',
          image: '/imagen/PACO.jpg',
          clicks: 0,
        },
        {
          name: 'Lupa',
          image: '/imagen/Lupa.jpg',
          clicks: 0,
        },
        {
          name: 'Liri',
          image: '/imagen/Liri.jpg',
          clicks: 0,
        },
        {
          name: 'Liko',
          image: '/imagen/Liko.jpg',
          clicks: 0,
        },
      ],
      selectedCatIndex: 0,
      isAdminOpen: false,
      form: {
        name: '',
        image: '',
        clicks: 0,
      },
    }
  },
  computed: {
    selectedCat() {
      return this.cats[this.selectedCatIndex]
    },
  },
  methods: {
    selectCat(index) {
      this.selectedCatIndex = index
      if (this.isAdminOpen) {
        this.fillAdminForm()
      }
    },
    incrementClicks() {
      this.selectedCat.clicks += 1
    },
    openAdmin() {
      this.fillAdminForm()
      this.isAdminOpen = true
    },
    fillAdminForm() {
      this.form = {
        name: this.selectedCat.name,
        image: this.selectedCat.image,
        clicks: this.selectedCat.clicks,
      }
    },
    cancelAdmin() {
      this.isAdminOpen = false
    },
    saveAdmin() {
      this.selectedCat.name = this.form.name.trim()
      this.selectedCat.image = this.form.image.trim()
      this.selectedCat.clicks = Number(this.form.clicks)
      this.isAdminOpen = false
    },
  },
}
</script>

<template>
  <main class="app-shell">
    <header class="page-header">
      <p class="eyebrow">La col·lecció felina</p>
      <h1>Cat Clicker <span>4</span></h1>
      <p class="intro">Tria un gat i compta quantes vegades et torna la mirada.</p>
    </header>

    <div class="cat-layout">
      <nav class="cat-sidebar" aria-label="Llista de gats">
        <h2>Els meus gats</h2>
        <ul class="cat-list">
          <li v-for="(cat, index) in cats" :key="index">
            <button
              class="cat-choice"
              :class="{ selected: selectedCatIndex === index }"
              type="button"
              :aria-current="selectedCatIndex === index ? 'true' : undefined"
              @click="selectCat(index)"
            >
              <span class="cat-choice-name">{{ cat.name }}</span>
              <span class="cat-choice-count">{{ cat.clicks }}</span>
            </button>
          </li>
        </ul>
        <p class="sidebar-note">Fes clic a la foto per sumar un clic.</p>
      </nav>

      <section class="cat-card" aria-live="polite">
        <div class="cat-photo-wrap">
          <button
            class="cat-photo-button"
            type="button"
            :aria-label="`Fer clic a la foto de ${selectedCat.name}`"
            @click="incrementClicks"
          >
            <img class="cat-photo" :src="selectedCat.image" :alt="selectedCat.name" />
            <span class="photo-hint">Clica'm!</span>
          </button>
        </div>
        <div class="cat-details">
          <div>
            <p class="eyebrow">Gat seleccionat</p>
            <h2>{{ selectedCat.name }}</h2>
          </div>
          <p class="click-total">
            <span class="click-number">{{ selectedCat.clicks }}</span>
            {{ selectedCat.clicks === 1 ? 'clic' : 'clics' }}
          </p>
        </div>
        <button class="admin-button" type="button" @click="openAdmin">
          <span aria-hidden="true">⚙</span> Administrar
        </button>
      </section>
    </div>

    <div v-if="isAdminOpen" class="modal-backdrop" @click.self="cancelAdmin">
      <form class="admin-panel" @submit.prevent="saveAdmin">
        <div class="admin-heading">
          <div>
            <p class="eyebrow">Configuració</p>
            <h2>Editar {{ selectedCat.name }}</h2>
          </div>
          <button
            class="close-button"
            type="button"
            aria-label="Tancar"
            @click="cancelAdmin"
          >
            ×
          </button>
        </div>

        <label class="form-field">
          <span>Nom</span>
          <input v-model="form.name" type="text" required />
        </label>
        <label class="form-field">
          <span>URL de la imatge</span>
          <input v-model="form.image" type="url" required />
        </label>
        <label class="form-field">
          <span>Nombre de clics</span>
          <input v-model.number="form.clicks" type="number" min="0" step="1" required />
        </label>

        <div class="form-actions">
          <button class="cancel-button" type="button" @click="cancelAdmin">Cancel·lar</button>
          <button class="save-button" type="submit">Desar canvis</button>
        </div>
      </form>
    </div>
  </main>
</template>
