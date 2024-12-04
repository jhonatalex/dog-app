<template>
  <div class="container my-5 text-center">
    <h2 class="mb-4">Imágenes de {{ capitalizedBreed }}</h2>
    <button @click="goBack" class="btn btn-secondary mb-4">← Volver a la lista</button>
    
    <!-- Grid de imágenes -->
    <div v-if="images.length" class="row">
      <div v-for="(image, index) in images" :key="index" class="col-md-4 mb-4">
        <div class="card shadow-sm" @click="openModal(image)">
          <img :src="image" class="card-img-top" alt="Imagen de perro" />
        </div>
      </div>
    </div>
    <p v-else>Cargando imágenes...</p>

    <!-- Modal de imagen -->
    <div v-if="showModal" class="modal fade show d-block" tabindex="-1" role="dialog">
      <div class="modal-dialog modal-dialog-centered">
        <div class="modal-content bg-dark text-light">
          <div class="modal-header">
            <h5 class="modal-title">Imagen de {{ capitalizedBreed }}</h5>
            <button type="button" class="btn-close" aria-label="Close" @click="closeModal"></button>
          </div>
          <div class="modal-body text-center">
            <img :src="selectedImage" class="img-fluid" alt="Imagen seleccionada" />
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-primary" @click="shareImage">Compartir</button>
            <button type="button" class="btn btn-secondary" @click="closeModal">Cerrar</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'DogDetail',
  props: ['breed'],
  data() {
    return {
      images: [],
      showModal: false,
      selectedImage: null,
    };
  },
  methods: {
    async fetchImages() {
      try {
        const response = await axios.get(`https://dog.ceo/api/breed/${this.breed}/images`);
        this.images = response.data.message;
      } catch (error) {
        console.error('Error al obtener las imágenes:', error);
      }
    },
    goBack() {
      this.$router.push('/');
    },
    openModal(image) {
      this.selectedImage = image;
      this.showModal = true;
    },
    closeModal() {
      this.showModal = false;
      this.selectedImage = null;
    },
    shareImage() {
      const subject = `Mira esta imagen de un ${this.capitalizedBreed}!`;
      const body = `Aquí tienes una imagen de un ${this.capitalizedBreed}: ${this.selectedImage}`;
      window.location.href = `mailto:?subject=${encodeURIComponent(subject)}&body=${encodeURIComponent(body)}`;
    },
  },
  computed: {
    capitalizedBreed() {
      return this.breed.charAt(0).toUpperCase() + this.breed.slice(1);
    },
  },
  created() {
    this.fetchImages();
  },
};
</script>

<style scoped>
.container {
  color: #ffffff;
}

.card {
  cursor: pointer;
  transition: transform 0.3s ease;
}
.card:hover {
  transform: scale(1.05);
}
.card-img-top {
  object-fit: cover;
  height: 200px;
}

.modal-content {
  background-color: #1e1e1e;
  color: #f5f5f5;
}
</style>
