<template>
  <div class="login-wrapper d-flex justify-content-center align-items-center">
    <div class="card shadow-sm" style="width: 400px;">
      <div class="card-body p-4">
        <h2 class="text-center mb-4 text-success">
          <i class="bi bi-mortarboard-fill me-2"></i>
          SISWA APP
        </h2>
        
        <form @submit.prevent="handleLogin" class="needs-validation" novalidate>
          <div class="mb-3">
            <label for="username" class="form-label">Username</label>
            <div class="input-group">
              <span class="input-group-text bg-success text-white">
                <i class="bi bi-person-fill"></i>
              </span>
              <input
                type="text"
                class="form-control"
                id="username"
                v-model="username"
                required
                placeholder="Masukkan username"
              >
            </div>
          </div>
          
          <div class="mb-4">
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
          
          <div class="d-grid gap-2">
            <button type="submit" class="btn btn-success">
              <i class="bi bi-box-arrow-in-right me-2"></i>
              Login
            </button>
            <button 
              type="button" 
              class="btn btn-outline-success"
              @click="goToRegister"
            >
              <i class="bi bi-person-plus me-2"></i>
              Register
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import Swal from 'sweetalert2'

const router = useRouter()
const username = ref('iran@gmail.com')
const password = ref('iran123')

onMounted(() => {
  // Check if already logged in
  const isLoggedIn = localStorage.getItem('isLoggedIn')
  if (isLoggedIn === 'true') {
    router.push('/')
  }
})

const handleLogin = async () => {
  // For demo purposes, we'll use a simple check
  if (username.value === 'iran@gmail.com' && password.value === 'iran123') {
    // Store login state
    localStorage.setItem('isLoggedIn', 'true')
    
    // Show success message
    await Swal.fire({
      icon: 'success',
      title: 'Login Berhasil!',
      text: 'Selamat datang di SISWA APP',
      timer: 2000,
      showConfirmButton: false
    })
    
    // Redirect to home
    router.push('/')
  } else {
    Swal.fire({
      icon: 'error',
      title: 'Login Gagal',
      text: 'Username atau password salah',
      confirmButtonColor: '#198754'
    })
  }
}

const goToRegister = () => {
  router.push('/register')
}
</script>

<style scoped>
.login-wrapper {
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