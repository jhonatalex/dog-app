<template>
  <div class="container my-5">
    <h1 class="text-center mb-4">Razas de Perros</h1>
    <div class="row">
      <div
        v-for="(breed, index) in breeds"
        :key="breed.name"
        class="col-md-4 mb-4"
        @click="goToDetail(breed.name)"
      >
        <div class="card h-100 shadow-sm ">
          <img :src="breed.image" class="card-img-top" alt="Imagen de raza" />
          <div class="card-body bg-gris">
            <h5 class="card-title text-center text-white">{{ breed.name }}</h5>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'DogList',
  data() {
    return {
      breeds: [],
    };
  },
  methods: {
    async fetchBreeds() {
      try {
        const response = await axios.get('https://dog.ceo/api/breeds/list/all');
        const breedNames = Object.keys(response.data.message);

        // Obtener imagen de vista previa para cada raza
        const breedData = await Promise.all(
          breedNames.map(async (breed) => {
            const imageResponse = await axios.get(`https://dog.ceo/api/breed/${breed}/images/random`);
            return { name: breed, image: imageResponse.data.message };
          })
        );
        this.breeds = breedData;
      } catch (error) {
        console.error('Error al obtener las razas:', error);
      }
    },
    goToDetail(breed) {
      this.$router.push({ name: 'DogDetail', params: { breed } });
    },
  },
  created() {
    this.fetchBreeds();
  },
};
</script>

<style scoped>
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

.bg-gris{
  background-color:#333

}
</style>
