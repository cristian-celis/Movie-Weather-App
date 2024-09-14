<template>
  <div class="container mx-auto p-4">
    <h1 class="text-center text-4xl font-bold m-4">Busca una pelicula y mira el clima de tu ciudad!!</h1>

    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
      <section class="p-4 border rounded-md shadow-md bg-white">
        <h2 class="text-xl font-semibold mb-2">Informacion del clima</h2>
        <div class="mb-4">
          <label for="city" class="block text-sm font-medium mb-1">Ingresa tu ciudad:</label>
          <input id="city" v-model="city" type="text" class="p-2 border border-gray-300 rounded-md w-full" placeholder="e.g., Bogota" />
        </div>
        
        <div v-if="weatherData" class="mt-4">
          <h3 class="text-lg font-semibold mb-2">{{ weatherData.location.name }}</h3>
          <p><span class="font-semibold">Temperatura:</span> {{ weatherData.current.temp_c }}°C</p>
          <p><span class="font-semibold">Clima:</span> <img :src="weatherData.current.condition.icon"/> {{ weatherData.current.condition.text }}</p>
          <p><span class="font-semibold">Humedad:</span> {{ weatherData.current.humidity }}%</p>
        </div>
      </section>
      
      <section class="p-4 border rounded-md shadow-md bg-white">
        <h2 class="text-xl font-semibold mb-2">Informacion de la pelicula</h2>
        <div class="mb-4">
          <label for="movie" class="block text-sm font-medium mb-1">Ingresa el titulo de la peli:</label>
          <input id="movie" v-model="movieTitle" type="text" class="p-2 border border-gray-300 rounded-md w-full" placeholder="e.g., Inception" />
        </div>
        
        <div v-if="movieData" class="mt-4">
          <h3 class="text-lg font-semibold mb-2">{{ movieData.Title }}</h3>
          <p><span class="font-semibold">Director:</span> {{ movieData.Director }}</p>
          <p><span class="font-semibold">Actores:</span> {{ movieData.Actors }}</p>
          <p><span class="font-semibold">Sipnosis:</span> {{ movieData.Plot }}</p>
          <p><span class="font-semibold">Puntaje IMDB:</span> {{ movieData.imdbRating }}</p>
          <img :src="movieData.Poster" class="mt-2 w-64" />
        </div>
      </section>
    </div>

    <!-- Botón para llamar al backend -->
    <div class="mt-4">
      <button @click="fetchData" class="bg-green-500 text-white p-2 rounded-md">Obtener Informacion</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      city: '',
      movieTitle: '',
      weatherData: null,
      movieData: null,
    };
  },
  methods: {
    async fetchData() {
      try {
        console.log("Llamando a la API")
        const response = await fetch('http://localhost:3000/fetch-data', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({
            city: this.city,
            movieTitle: this.movieTitle
          }),
        });

        console.log("Finalizo llamada")

        if (!response.ok) {
          throw new Error('La llamada no fue exitosa.');
        }

        const data = await response.json();
        console.log('Respuesta:', data);
        
        this.weatherData = data.weather;
        this.movieData = data.movie;
      } catch (error) {
        console.error('Error al llamar al backend:', error);
      }
    }
  }
}
</script>