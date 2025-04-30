<template>
  <nav class="navbar navbar-expand-lg navbar-dark" :class="isDarkMode ? 'bg-dark' : 'bg-success'">
    <div class="container">
      <a class="navbar-brand d-flex align-items-center" href="#">
        <i class="bi bi-mortarboard-fill me-2 fs-4"></i>
        <span class="fw-bold">EduPedia</span>
      </a>
      
      <div class="d-flex align-items-center">
        <div class="form-check form-switch me-3">
          <input 
            class="form-check-input" 
            type="checkbox" 
            id="darkModeSwitch"
            v-model="isDarkMode"
            @change="toggleDarkMode"
          >
          <label class="form-check-label" :class="isDarkMode ? 'text-light' : 'text-white'" for="darkModeSwitch">
            <i class="bi" :class="isDarkMode ? 'bi-moon-fill' : 'bi-sun-fill'"></i>
          </label>
        </div>
        
        <div class="dropdown">
          <button 
            class="btn d-flex align-items-center p-0 profile-btn" 
            :class="isDarkMode ? 'text-light' : 'text-white'"
            type="button" 
            data-bs-toggle="dropdown" 
            aria-expanded="false"
          >
            <img 
              src="https://ui-avatars.com/api/?name=Iran&background=198754&color=fff" 
              alt="Profile" 
              class="rounded-circle me-2"
              style="width: 32px; height: 32px; object-fit: cover;"
            >
            <span>Iran</span>
          </button>
          <ul class="dropdown-menu dropdown-menu-end">
            <li>
              <a class="dropdown-item" href="#" @click="handleLogout">
                <i class="bi bi-box-arrow-right me-2"></i>
                Logout
              </a>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </nav>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import Swal from 'sweetalert2'

const router = useRouter()
const isDarkMode = ref(false)

onMounted(() => {
  // Check if dark mode was previously enabled
  const savedMode = localStorage.getItem('darkMode')
  if (savedMode === 'true') {
    isDarkMode.value = true
    document.body.classList.add('dark-mode')
  }
})

const toggleDarkMode = () => {
  if (isDarkMode.value) {
    document.body.classList.add('dark-mode')
    localStorage.setItem('darkMode', 'true')
  } else {
    document.body.classList.remove('dark-mode')
    localStorage.setItem('darkMode', 'false')
  }
}

const handleLogout = async () => {
  const result = await Swal.fire({
    title: 'Yakin ingin logout?',
    text: "Anda akan keluar dari aplikasi",
    icon: 'question',
    showCancelButton: true,
    confirmButtonColor: '#198754',
    cancelButtonColor: '#6c757d',
    confirmButtonText: 'Ya, Logout',
    cancelButtonText: 'Batal'
  })

  if (result.isConfirmed) {
    // Clear login state
    localStorage.removeItem('isLoggedIn')
    // Keep dark mode preference
    router.push('/login')
  }
}
</script>

<style scoped>
.navbar {
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  transition: all 0.3s ease;
}

.form-check-input:checked {
  background-color: #198754;
  border-color: #198754;
}

:root {
  --dark-bg: #1a1a1a;
  --dark-text: #ffffff;
  --dark-text-secondary: #e0e0e0;
  --dark-text-muted: #b0b0b0;
  --dark-card: #2d2d2d;
  --dark-border: #404040;
  --dark-input: #333333;
  --dark-hover: #404040;
  --dark-success: #198754;
  --dark-warning: #ffc107;
  --dark-danger: #dc3545;
  --dark-shadow: rgba(0, 0, 0, 0.3);
}

.dark-mode {
  background-color: var(--dark-bg);
  color: var(--dark-text);
}

.dark-mode .container {
  background-color: var(--dark-card);
  color: var(--dark-text);
  border: 1px solid var(--dark-border);
}

.dark-mode .table {
  background-color: var(--dark-card);
  color: var(--dark-text);
}

.dark-mode .table td,
.dark-mode .table th {
  color: var(--dark-text);
  border-color: var(--dark-border);
  background-color: var(--dark-card);
}

.dark-mode .table th {
  font-weight: 600;
  letter-spacing: 0.5px;
  background-color: var(--dark-hover);
}

.dark-mode .table td {
  font-weight: 400;
  background-color: var(--dark-card);
}

.dark-mode .table-striped > tbody > tr:nth-of-type(odd) {
  background-color: var(--dark-hover);
}

.dark-mode .table-hover tbody tr:hover {
  background-color: var(--dark-hover);
  color: var(--dark-text);
}

.dark-mode .btn-outline-light:hover {
  background-color: var(--dark-hover);
  color: var(--dark-text);
}

.dark-mode .text-muted {
  color: #adb5bd;
}

.dark-mode .btn-warning {
  background-color: var(--dark-warning);
  border-color: var(--dark-warning);
  color: #000;
  text-shadow: 0 1px 1px rgba(255, 255, 255, 0.3);
}

.dark-mode .btn-danger {
  background-color: var(--dark-danger);
  border-color: var(--dark-danger);
  color: #fff;
  text-shadow: 0 1px 1px rgba(0, 0, 0, 0.3);
}

.dark-mode .btn-success {
  background-color: var(--dark-success);
  border-color: var(--dark-success);
  color: #fff;
  text-shadow: 0 1px 1px rgba(0, 0, 0, 0.3);
}

.dark-mode .main-header {
  background: linear-gradient(135deg, #1a1a1a, #2d2d2d);
  color: var(--dark-text);
  text-shadow: 0 2px 4px var(--dark-shadow);
}

.dark-mode .form-section {
  background: linear-gradient(135deg, #1a1a1a, #2d2d2d);
  color: var(--dark-text);
}

.dark-mode .table-header {
  background: linear-gradient(135deg, #1a1a1a, #2d2d2d);
  color: var(--dark-text);
  text-shadow: 0 1px 2px var(--dark-shadow);
}

.dark-mode .main-wrapper {
  background: linear-gradient(135deg, #1a1a1a, #2d2d2d, #1a1a1a);
}

.profile-btn {
  transition: all 0.2s ease;
}

.profile-btn:hover {
  opacity: 0.8;
  transform: scale(1.05);
}

.profile-btn:active {
  transform: scale(0.95);
}
</style> 