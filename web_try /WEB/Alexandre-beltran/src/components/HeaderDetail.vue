<template>
  <div class="header-detail">
    <button class="close-btn" @click="$emit('close')">Fermer</button>
    <div v-if="loading" class="loading">Chargement...</div>
    <div v-else-if="error" class="error">Erreur : {{ error }}</div>
    <div v-else-if="header">
      <h2>{{ header.attributes.title || 'Sans titre' }}</h2>
      <div class="images">
        <img
          v-for="(img, idx) in images"
          :key="idx"
          :src="img"
          class="header-image"
          alt="Image de la galerie"
        />
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'HeaderDetail',
  props: {
    headerId: {
      type: [String, Number],
      required: true
    }
  },
  data() {
    return {
      header: null,
      images: [],
      loading: true,
      error: null
    }
  },
  async mounted() {
    try {
      const strapiUrl = import.meta.env.VITE_STRAPI_URL || 'http://localhost:1337'
      const response = await axios.get(`${strapiUrl}/api/headers/${this.headerId}?populate=*`)
      const header = response.data.data
      this.header = header
      // Récupérer les images de la galerie comme dans Main.vue (accès direct)
      if (header && header.attributes && header.attributes.galleris && header.attributes.galleris.data) {
        this.images = header.attributes.galleris.data.map(img =>
          img.attributes.url ? strapiUrl + img.attributes.url : 'https://via.placeholder.com/200'
        )
      } else {
        this.images = []
      }
      this.loading = false
    } catch (e) {
      this.error = e.response?.data?.message || e.message || 'Erreur inconnue'
      this.loading = false
    }
  }
}
</script>

<style scoped>
.header-detail {
  padding: 2rem;
  color: #c2c2c2;
}
.images {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  margin-top: 1rem;
}
.header-image {
  max-width: 300px;
  max-height: 300px;
  object-fit: contain;
  box-shadow: 0 0 10px rgba(0,0,0,0.3);
}
.close-btn {
  background: none;
  border: 1px solid #c2c2c2;
  color: #c2c2c2;
  padding: 0.5rem 1rem;
  cursor: pointer;
  margin-bottom: 1rem;
}
</style> 