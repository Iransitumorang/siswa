<template>
  <div class="register-wrapper d-flex justify-content-center align-items-center">
    <div class="card shadow-sm" style="width: 400px;">
      <div class="card-body p-4">
        <h2 class="text-center mb-4 text-success">
          <i class="bi bi-person-plus-fill me-2"></i>
          Register
        </h2>
        
        <form @submit.prevent="handleRegister" class="needs-validation" novalidate>
          <div class="mb-3">
            <label for="name" class="form-label">Nama Lengkap</label>
            <div class="input-group">
              <span class="input-group-text bg-success text-white">
                <i class="bi bi-person-fill"></i>
              </span>
              <input
                type="text"
                class="form-control"
                id="name"
                v-model="name"
                required
                placeholder="Masukkan nama lengkap"
              >
            </div>
          </div>

          <div class="mb-3">
            <label for="email" class="form-label">Email</label>
            <div class="input-group">
              <span class="input-group-text bg-success text-white">
                <i class="bi bi-envelope-fill"></i>
              </span>
              <input
                type="email"
                class="form-control"
                id="email"
                v-model="email"
                required
                placeholder="Masukkan email"
              >
            </div>
          </div>
          
          <div class="mb-3">
            <label for="password" class="form-label">Password</label>
            <div class="input-group">
              <span class="input-group-text bg-success text-white">
                <i class="bi bi-key-fill"></i>
              </span>
              <input
                type="password"
                class="form-control"
                id="password"
                v-model="password"
                required
                placeholder="Masukkan password"
              >
            </div>
          </div>

          <div class="mb-4">
            <label for="confirmPassword" class="form-label">Konfirmasi Password</label>
            <div class="input-group">
              <span class="input-group-text bg-success text-white">
                <i class="bi bi-key-fill"></i>
              </span>
              <input
                type="password"
                class="form-control"
                id="confirmPassword"
                v-model="confirmPassword"
                required
                placeholder="Konfirmasi password"
              >
            </div>
          </div>
          
          <div class="d-grid gap-2">
            <button type="submit" class="btn btn-success">
              <i class="bi bi-person-plus me-2"></i>
              Register
            </button>
            <button 
              type="button" 
              class="btn btn-outline-success"
              @click="goToLogin"
            >
              <i class="bi bi-arrow-left me-2"></i>
              Kembali ke Login
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import Swal from 'sweetalert2'

const router = useRouter()
const name = ref('')
const email = ref('')
const password = ref('')
const confirmPassword = ref('')

const handleRegister = async () => {
  if (password.value !== confirmPassword.value) {
    Swal.fire({
      icon: 'error',
      title: 'Gagal Register',
      text: 'Password dan konfirmasi password tidak sama',
      confirmButtonColor: '#198754'
    })
    return
  }

  // For demo purposes, we'll just show a success message
  await Swal.fire({
    icon: 'success',
    title: 'Register Berhasil!',
    text: 'Silakan login dengan akun yang baru dibuat',
    confirmButtonColor: '#198754'
  })
  
  router.push('/login')
}

const goToLogin = () => {
  router.push('/login')
}
</script>

<style scoped>
.register-wrapper {
  min-height: 100vh;
  background: linear-gradient(135deg, #2b8a3e, #40c057, #2b8a3e);
  background-size: 400% 400%;
  animation: gradientFlow 15s ease infinite;
}

@keyframes gradientFlow {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}

.card {
  border: none;
  border-radius: 15px;
}

.input-group-text {
  border: none;
}

.form-control {
  border-left: none;
}

.form-control:focus {
  box-shadow: none;
  border-color: #ced4da;
}

.btn-outline-success {
  border-color: #198754;
  color: #198754;
}

.btn-outline-success:hover {
  background-color: #198754;
  color: white;
}

.dark-mode .card {
  background-color: var(--dark-card) !important;
  border-color: var(--dark-border) !important;
  color: var(--dark-text) !important;
}

.dark-mode .form-control {
  background-color: var(--dark-input) !important;
  color: var(--dark-text) !important;
  border-color: var(--dark-border) !important;
}

.dark-mode .form-control:focus {
  background-color: var(--dark-input) !important;
  color: var(--dark-text) !important;
  border-color: var(--dark-success) !important;
}

.dark-mode .input-group-text {
  background-color: var(--dark-input) !important;
  color: var(--dark-text) !important;
  border-color: var(--dark-border) !important;
}

.dark-mode .btn-outline-success {
  border-color: var(--dark-success) !important;
  color: var(--dark-success) !important;
}

.dark-mode .btn-outline-success:hover {
  background-color: var(--dark-success) !important;
  color: white !important;
}
</style> 