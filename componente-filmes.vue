<template>
  <div class="movie-manager">
    <h2>Cadastro de Filmes</h2>
    
    <div class="form-container">
      <div class="form-group">
        <label for="title">Título:</label>
        <input 
          type="text" 
          id="title" 
          v-model="newMovie.title"
          :class="{ 'error': errors.title }"
        >
        <span class="error-message" v-if="errors.title">{{ errors.title }}</span>
      </div>

      <div class="form-group">
        <label for="director">Diretor:</label>
        <input 
          type="text" 
          id="director" 
          v-model="newMovie.director"
          :class="{ 'error': errors.director }"
        >
        <span class="error-message" v-if="errors.director">{{ errors.director }}</span>
      </div>

      <div class="form-group">
        <label for="year">Ano:</label>
        <input 
          type="number" 
          id="year" 
          v-model="newMovie.year"
          :class="{ 'error': errors.year }"
        >
        <span class="error-message" v-if="errors.year">{{ errors.year }}</span>
      </div>

      <div class="form-group">
        <label>Nota:</label>
        <div class="star-rating">
          <span 
            v-for="star in 5" 
            :key="star"
            @click="setRating(star)"
            :class="{ 'star-filled': star <= newMovie.rating, 'star-empty': star > newMovie.rating }"
            class="star"
          >
            ★
          </span>
        </div>
        <span class="error-message" v-if="errors.rating">{{ errors.rating }}</span>
      </div>

      <button @click="addMovie" class="btn-add">Cadastrar Filme</button>
    </div>

    <div class="movies-list">
      <div v-if="movies.length === 0" class="empty-message">
        Nenhum filme cadastrado ainda. Adicione seu primeiro filme!
      </div>
      
      <div v-else class="movie-card" v-for="(movie, index) in movies" :key="index">
        <div class="movie-info">
          <h3>{{ movie.title }}</h3>
          <p><strong>Diretor:</strong> {{ movie.director }}</p>
          <p><strong>Ano:</strong> {{ movie.year }}</p>
          <div class="star-rating">
            <span 
              v-for="star in 5" 
              :key="star"
              :class="{ 'star-filled': star <= movie.rating, 'star-empty': star > movie.rating }"
              class="star"
            >
              ★
            </span>
          </div>
        </div>
        <button @click="deleteMovie(index)" class="btn-delete">Excluir</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MovieManager',
  data() {
    return {
      movies: [],
      newMovie: {
        title: '',
        director: '',
        year: '',
        rating: 0
      },
      errors: {
        title: '',
        director: '',
        year: '',
        rating: ''
      }
    }
  },
  methods: {
    setRating(rating) {
      this.newMovie.rating = rating
      this.errors.rating = ''
    },

    validateForm() {
      let isValid = true
      this.errors = {
        title: '',
        director: '',
        year: '',
        rating: ''
      }

      if (!this.newMovie.title.trim()) {
        this.errors.title = 'O título é obrigatório'
        isValid = false
      }

      if (!this.newMovie.director.trim()) {
        this.errors.director = 'O diretor é obrigatório'
        isValid = false
      }

      if (!this.newMovie.year) {
        this.errors.year = 'O ano é obrigatório'
        isValid = false
      } else if (this.newMovie.year < 1888 || this.newMovie.year > new Date().getFullYear()) {
        this.errors.year = 'Insira um ano válido'
        isValid = false
      }

      if (this.newMovie.rating === 0) {
        this.errors.rating = 'Avalie o filme'
        isValid = false
      }

      return isValid
    },

    addMovie() {
      if (this.validateForm()) {
        this.movies.push({...this.newMovie})
        this.clearForm()
      }
    },

    deleteMovie(index) {
      this.movies.splice(index, 1)
    },

    clearForm() {
      this.newMovie = {
        title: '',
        director: '',
        year: '',
        rating: 0
      }
      this.errors = {
        title: '',
        director: '',
        year: '',
        rating: ''
      }
    }
  }
}
</script>

<style scoped>
.movie-manager {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.form-container {
  background-color: #f5f5f5;
  padding: 20px;
  border-radius: 8px;
  margin-bottom: 20px;
}

.form-group {
  margin-bottom: 15px;
}

label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
}

input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

input.error {
  border-color: #ff4444;
}

.error-message {
  color: #ff4444;
  font-size: 0.8em;
  margin-top: 5px;
}

.star-rating {
  display: flex;
  gap: 5px;
  margin: 5px 0;
}

.star {
  font-size: 24px;
  cursor: pointer;
  transition: color 0.2s;
}

.star-empty {
  color: #ccc;
}

.star-filled {
  color: #ffd700;
}

.btn-add {
  background-color: #4CAF50;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.btn-add:hover {
  background-color: #45a049;
}

.empty-message {
  text-align: center;
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 8px;
  color: #6c757d;
}

.movie-card {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: white;
  padding: 15px;
  margin-bottom: 10px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.movie-info {
  flex-grow: 1;
}

.movie-info h3 {
  margin: 0 0 10px 0;
}

.movie-info p {
  margin: 5px 0;
}

.btn-delete {
  background-color: #ff4444;
  color: white;
  padding: 8px 15px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.btn-delete:hover {
  background-color: #cc0000;
}
</style>
