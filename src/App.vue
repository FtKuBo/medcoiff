<script setup lang="ts">
import { ref } from 'vue'

const showConfirmation = ref(false)
const form = ref<HTMLFormElement | null>(null)

const handleSubmit = (event: Event) => {
  event.preventDefault()
  showConfirmation.value = true
  // Clear the form
  if (form.value) {
    form.value.reset()
  }
}

const closeConfirmation = () => {
  showConfirmation.value = false
}
</script>

<template>
  <div class="app">
    <!-- Navigation -->
    <nav class="navbar navbar-expand-lg navbar-light bg-light fixed-top">
      <div class="container">
        <a class="navbar-brand" href="#">MedCoiff</a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
          <ul class="navbar-nav ms-auto">
            <li class="nav-item">
              <a class="nav-link" href="#accueil">Accueil</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="#services">Services</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="#rendez-vous">Rendez-vous</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="#a-propos">À propos</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="#contact">Contact</a>
            </li>
          </ul>
        </div>
      </div>
    </nav>

    <!-- Hero Section -->
    <section id="accueil" class="hero-section">
      <div class="container h-100">
        <div class="row h-100 align-items-center">
          <div class="col-lg-6 hero-content">
            <h1 class="display-3 fw-bold mb-4">Bienvenue chez <span class="brand-highlight">MedCoiff</span></h1>
            <p class="lead mb-4">Votre style, notre passion. Découvrez l'excellence de la coiffure.</p>
            <div class="hero-features mb-5">
              <div class="feature-item">
                <i class="fas fa-cut"></i>
                <span>Coupes tendance</span>
              </div>
              <div class="feature-item">
                <i class="fas fa-paint-brush"></i>
                <span>Coloration experte</span>
              </div>
              <div class="feature-item">
                <i class="fas fa-star"></i>
                <span>Service premium</span>
              </div>
            </div>
            <div class="hero-buttons">
              <a href="#rendez-vous" class="btn btn-primary btn-lg me-3">
                <i class="fas fa-calendar-alt me-2"></i>Prendre rendez-vous
              </a>
              <a href="#services" class="btn btn-outline-light btn-lg">
                <i class="fas fa-list me-2"></i>Nos services
              </a>
            </div>
          </div>
          <div class="col-lg-6 hero-image-container d-none d-lg-block">
            <div class="hero-image">
              <img src="https://images.unsplash.com/photo-1562322140-8baeececf3df?ixlib=rb-1.2.1&auto=format&fit=crop&w=800&q=80" alt="Coiffure professionnelle" class="img-fluid rounded-3">
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Services Section -->
    <section id="services" class="py-5">
      <div class="container">
        <h2 class="text-center mb-5">Nos Services</h2>
        <div class="row">
          <div class="col-md-4 mb-4">
            <div class="card">
              <div class="card-body">
                <h5 class="card-title">Coupe de cheveux</h5>
                <p class="card-text">Coupe personnalisée selon vos envies et votre style.</p>
                <p class="price">À partir de 25€</p>
              </div>
            </div>
          </div>
          <div class="col-md-4 mb-4">
            <div class="card">
              <div class="card-body">
                <h5 class="card-title">Coloration</h5>
                <p class="card-text">Coloration professionnelle pour sublimer votre style.</p>
                <p class="price">À partir de 45€</p>
              </div>
            </div>
          </div>
          <div class="col-md-4 mb-4">
            <div class="card">
              <div class="card-body">
                <h5 class="card-title">Brushing</h5>
                <p class="card-text">Mise en beauté pour toutes les occasions.</p>
                <p class="price">À partir de 35€</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Appointment Section -->
    <section id="rendez-vous" class="py-5 bg-light">
      <div class="container">
        <h2 class="text-center mb-5">Prendre rendez-vous</h2>
        <div class="row justify-content-center">
          <div class="col-md-8">
            <div class="card">
              <div class="card-body">
                <form @submit="handleSubmit" ref="form">
                  <div class="mb-3">
                    <label for="name" class="form-label">Nom complet</label>
                    <input type="text" class="form-control" id="name" required>
                  </div>
                  <div class="mb-3">
                    <label for="email" class="form-label">Email</label>
                    <input type="email" class="form-control" id="email" required>
                  </div>
                  <div class="mb-3">
                    <label for="phone" class="form-label">Téléphone</label>
                    <input type="tel" class="form-control" id="phone" required>
                  </div>
                  <div class="mb-3">
                    <label for="service" class="form-label">Service souhaité</label>
                    <select class="form-select" id="service" required>
                      <option value="">Choisir un service</option>
                      <option value="coupe">Coupe de cheveux</option>
                      <option value="coloration">Coloration</option>
                      <option value="brushing">Brushing</option>
                    </select>
                  </div>
                  <div class="mb-3">
                    <label for="date" class="form-label">Date souhaitée</label>
                    <input type="date" class="form-control" id="date" required>
                  </div>
                  <div class="mb-3">
                    <label for="time" class="form-label">Heure souhaitée</label>
                    <input type="time" class="form-control" id="time" required>
                  </div>
                  <button type="submit" class="btn btn-primary w-100">Réserver</button>
                </form>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- About Section -->
    <section id="a-propos" class="py-5">
      <div class="container">
        <h2 class="text-center mb-5">À propos de nous</h2>
        <div class="row align-items-center">
          <div class="col-md-6">
            <p class="lead">MedCoiff est un salon de coiffure moderne où nous mettons l'accent sur la qualité et le service personnalisé.</p>
            <p>Notre équipe de coiffeurs professionnels est là pour vous offrir une expérience unique et vous aider à exprimer votre style.</p>
          </div>
          <div class="col-md-6">
            <div class="card">
              <div class="card-body">
                <h5 class="card-title">Horaires d'ouverture</h5>
                <ul class="list-unstyled">
                  <li>Lundi - Vendredi: 9h00 - 19h00</li>
                  <li>Samedi: 9h00 - 18h00</li>
                  <li>Dimanche: Fermé</li>
                </ul>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Contact Section -->
    <section id="contact" class="py-5 bg-light">
      <div class="container">
        <h2 class="text-center mb-5">Contact</h2>
        <div class="row">
          <div class="col-md-6 mb-4">
            <div class="card">
              <div class="card-body">
                <h5 class="card-title">Informations de contact</h5>
                <p><i class="fas fa-map-marker-alt"></i> 123 Rue de la Coiffure, 75000 Paris</p>
                <p><i class="fas fa-phone"></i> +33 1 23 45 67 89</p>
                <p><i class="fas fa-envelope"></i> contact@medcoiff.fr</p>
              </div>
            </div>
          </div>
          <div class="col-md-6">
            <div class="card">
              <div class="card-body">
                <h5 class="card-title">Nous suivre</h5>
                <div class="social-links">
                  <a href="#" class="btn btn-outline-primary me-2"><i class="fab fa-facebook"></i></a>
                  <a href="#" class="btn btn-outline-primary me-2"><i class="fab fa-instagram"></i></a>
                  <a href="#" class="btn btn-outline-primary"><i class="fab fa-twitter"></i></a>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Footer -->
    <footer class="bg-dark text-light py-4">
      <div class="container text-center">
        <p>&copy; 2024 MedCoiff. Tous droits réservés.</p>
      </div>
    </footer>

    <!-- Confirmation Message -->
    <div v-if="showConfirmation" class="confirmation-overlay" @click="closeConfirmation">
      <div class="confirmation-message" @click.stop>
        <button class="close-button" @click="closeConfirmation">
          <i class="fas fa-times"></i>
        </button>
        <div class="confirmation-content">
          <i class="fas fa-check-circle"></i>
          <h3>Réservation confirmée !</h3>
          <p>Nous vous contacterons bientôt pour confirmer votre rendez-vous.</p>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.hero-section {
  height: 100vh;
  background: linear-gradient(rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5)), url('/src/assets/salon-bg.jpg');
  background-size: cover;
  background-position: center;
  color: white;
  padding-top: 80px;
}

.navbar {
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.card {
  transition: transform 0.3s ease;
  border: none;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.card:hover {
  transform: translateY(-5px);
}

.price {
  font-weight: bold;
  color: #0d6efd;
  margin-top: 1rem;
}

.social-links {
  margin-top: 1rem;
}

section {
  scroll-margin-top: 80px;
}

.confirmation-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(5px);
  z-index: 999;
  display: flex;
  justify-content: center;
  align-items: center;
  animation: fadeIn 0.3s ease-out;
}

.confirmation-message {
  position: relative;
  background: white;
  padding: 2rem;
  border-radius: 10px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
  z-index: 1000;
  animation: slideIn 0.3s ease-out;
}

.close-button {
  position: absolute;
  top: 10px;
  right: 10px;
  background: none;
  border: none;
  font-size: 1.2rem;
  color: #6c757d;
  cursor: pointer;
  padding: 5px;
  transition: color 0.3s ease;
}

.close-button:hover {
  color: #dc3545;
}

.confirmation-content {
  text-align: center;
  padding: 1rem;
}

.confirmation-content i {
  font-size: 3rem;
  color: #28a745;
  margin-bottom: 1rem;
}

.confirmation-content h3 {
  color: #28a745;
  margin-bottom: 0.5rem;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateY(-20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes slideOut {
  from {
    opacity: 1;
    transform: translateY(0);
  }
  to {
    opacity: 0;
    transform: translateY(-20px);
  }
}

.confirmation-overlay.v-leave-active {
  animation: fadeIn 0.3s ease-out reverse;
}

.confirmation-message.v-leave-active {
  animation: slideOut 0.3s ease-out;
}
</style>
