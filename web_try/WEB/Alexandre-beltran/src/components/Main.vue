<template>
  <div class="main-layout" :class="{ 'blur-active': hoveredHeaderId && !selectedHeaderId }">
    <div v-if="selectedHeaderId">
      <HeaderDetail :header-id="selectedHeaderId" @close="selectedHeaderId = null" />
    </div>
    <template v-else>
      <!-- Colonne gauche : les items -->
      <div class="items-column">
        <div v-if="loading" class="loading">
          <div class="item">
            <div class="element">Chargement...</div>
            <div class="element">...</div>
            <div class="element">...</div>
          </div>
        </div>

        <div v-else-if="error" class="error">
          <div class="item">
            <div class="element">Erreur de connexion</div>
            <div class="element">{{ error }}</div>
            <div class="element">Vérifiez Strapi</div>
          </div>
        </div>

        <div v-else>
          <div
            class="item"
            v-for="header in headers"
            :key="header.id"
            @mouseenter="hoveredHeaderId = header.id"
            @mouseleave="hoveredHeaderId = null"
            @click="selectedHeaderId = header.id"
            :class="{ hovered: hoveredHeaderId === header.id, 'no-blur': hoveredHeaderId === header.id }"
          >
            <div class="element">{{ header.title }}</div>
            <div class="element year">{{ header.year }}</div>
            <div class="element">
              {{ header.fraction }}
              <span v-if="hoveredHeaderId === header.id" class="hover-dot"></span>
            </div>
          </div>
        </div>
      </div>

      <!-- Colonne droite : image fixe -->
      <div class="image-column no-blur">
        <img
          v-if="hoveredHeaderId"
          :src="getHeaderImage(hoveredHeaderId)"
          class="fixed-image"
          alt="Preview"
        />
      </div>

      <!-- Menu -->
      <div class="menu-wrapper">
        <Menu />
      </div>
    </template>
  </div>
</template>

<script>
import axios from 'axios'
import Menu from './menu.vue'
import HeaderDetail from './HeaderDetail.vue'

export default {
  name: 'MainComponent',
  components: { Menu, HeaderDetail },
  data() {
    return {
      headers: [],
      loading: true,
      error: null,
      hoveredHeaderId: null,
      selectedHeaderId: null
    }
  },

  async mounted() {
    await this.loadHeaders()
  },

  methods: {
    async loadHeaders() {
      try {
        const strapiUrl = import.meta.env.VITE_STRAPI_URL || 'http://localhost:1337'
        const response = await axios.get(`${strapiUrl}/api/headers?sort=order:asc&populate=*`)

        this.headers = response.data.data.map(item => ({
          id: item.id,
          title: item.title || 'Sans titre',
          year: item.Year || '-',
          fraction: item.fraction || '-',
          order: item.order || 0,
          image: item.image?.url
            ? strapiUrl + item.image.url
            : 'https://via.placeholder.com/200'
        }))

        this.loading = false
      } catch (error) {
        this.error = error.response?.data?.message || error.message || 'Erreur inconnue'
        this.loading = false
      }
    },

    getHeaderImage(id) {
      const header = this.headers.find(h => h.id === id)
      return header?.image || 'https://via.placeholder.com/200'
    }
  }
}
</script>

<style scoped>
.main-layout {
  display: flex;
  flex-direction: row;
  width: 100vw;
  height: 100vh;
  font-family: "Barlow-Condesed-R";
  color: rgb(194, 194, 194);
}

.items-column {
  width: 50%;
  padding: 2rem;
  display: flex;
  flex-direction: column;
  gap: 1rem;
  overflow-y: auto;
}

.year{
    padding-left: 5rem;
}

.image-column {
  width: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 2rem;
}


.fixed-image {
  width: auto;
  height: 40vh;
  object-fit: contain;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
  pointer-events: none;
}

.item {
  display: flex;
  justify-content: space-between;
  transition: background-color 0.3s;
  border-bottom: 1px solid rgb(80, 80, 80);
}


.item.hovered {
  background-color: rgba(255, 255, 255, 0.05);
}

.element {
  flex: 1;
  font-size: 1.2rem;
  font-weight: 500;
}

.element .hovered{
    color: green !important;
}

.hover-dot {
  display: inline-block;
  margin-left: 5.5rem;
  width: 10px;
  height: 10px;
  background-color: rgb(194, 194, 194);
  border-radius: 50%;
  vertical-align: middle;
  animation: fadeIn 0.2s ease-in-out;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: scale(0.5);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}

/* Applique un flou général sauf sur les éléments avec .no-blur */
.blur-active .items-column .item:not(.no-blur),
.blur-active .image-column {
  filter: blur(2px);
  transition: filter 0.3s ease;
}

/* Désactive le flou pour l’élément actif */
.item.no-blur {
  filter: none !important;
  position: relative;
  z-index: 1;
}

.blur-active .image-column{
    filter: none !important;
}

/* Flouter le menu aussi */
.blur-active .menu-wrapper {
  filter: blur(4px);
  transition: filter 0.3s ease;
}


/* Responsive mobile */
@media screen and (max-width: 768px) {
  .main-layout {
    flex-direction: column;
  }

  .items-column,
  .image-column {
    width: 100%;
    padding: 1rem;
  }

  .fixed-image {
    max-height: 40vh;
  }
}
</style>

