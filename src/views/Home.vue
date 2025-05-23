<template>
  <div>
    <AppHeader />
    <div class="main-wrapper py-4">
      <div class="container bg-white rounded shadow p-4">
        <header class="text-center mb-4 main-header">
          <h1 class="fw-bold text-white">
            <i class="bi bi-people-fill me-2"></i>Platform Administrasi Sekolah
          </h1>
          <p class="text-white">
            <i class="bi bi-gear-wide-connected me-1"></i>Sistem manajemen data siswa menggunakan Vue.js dan Quarkus
          </p>
        </header>

        <SiswaForm
          :siswaEdit="siswaEdit"
          @siswa-added="getData"
          @cancel-edit="siswaEdit = null"
          class="form-section"
        />

        <div class="row mb-3">
          <div class="col-md-6"></div>
          <div class="col-md-6 d-flex justify-content-end">
            <div class="input-group me-3" style="width: 300px">
              <input 
                type="text" 
                class="form-control" 
                placeholder="Cari siswa..." 
                v-model="searchQuery"
                @input="applySearch"
              >
              <button 
                v-if="searchQuery" 
                class="btn btn-outline-secondary" 
                type="button" 
                @click="clearSearch"
              >
                <i class="bi bi-x"></i>
              </button>
              <button 
                class="btn btn-success" 
                type="button"
              >
                <i class="bi bi-search"></i>
              </button>
            </div>
            
            <select v-model="sortKey" class="form-select w-auto me-2" @change="applySort">
              <option value="">Sort By</option>
              <option value="nama">Nama</option>
              <option value="alamat">Alamat</option>
              <option value="umur">Umur</option>
            </select>
            <button class="btn btn-outline-success btn-sm" @click="toggleSortOrder">
              <i class="bi" :class="sortOrder === 'asc' ? 'bi-sort-alpha-down' : 'bi-sort-alpha-up'"></i>
            </button>
          </div>
        </div>

        <div class="table-responsive">
          <table class="table table-bordered table-striped table-hover">
            <thead class="table-header text-center align-middle">
              <tr>
                <th><i class="bi bi-hash me-1"></i>No</th>
                <th><i class="bi bi-person-fill me-1"></i>Nama</th>
                <th><i class="bi bi-house-door-fill me-1"></i>Alamat</th>
                <th><i class="bi bi-calendar3 me-1"></i>Umur</th>
                <th><i class="bi bi-activity me-1"></i>Aksi</th>
              </tr>
            </thead>
            <tbody class="align-middle text-center">
              <tr v-for="(siswa, index) in sortedAndFilteredSiswa" :key="siswa.id">
                <td>{{ index + 1 }}</td>
                <td>{{ capitalizeWords(siswa.nama) }}</td>
                <td>{{ capitalizeWords(siswa.alamat) }}</td>
                <td>{{ siswa.umur }}</td>
                <td>
                  <button
                    @click="editSiswa(siswa)"
                    class="btn btn-sm btn-warning text-white me-2"
                  >
                    <i class="bi bi-pencil-square"></i>
                  </button>
                  <button
                    @click="hapusSiswa(siswa.id)"
                    class="btn btn-sm btn-danger"
                  >
                    <i class="bi bi-trash-fill"></i>
                  </button>
                </td>
              </tr>
              <tr v-if="sortedAndFilteredSiswa.length === 0">
                <td colspan="5" class="text-muted">
                  <i class="bi bi-exclamation-circle me-1"></i>Tidak ditemukan data siswa
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Swal from 'sweetalert2'
import SiswaForm from '../components/SiswaForm.vue'
import AppHeader from '../components/AppHeader.vue'

const API_URL = 'http://localhost:8081/siswa'

export default {
  components: {
    SiswaForm,
    AppHeader
  },
  data() {
    return {
      siswa: [],
      siswaEdit: null,
      sortKey: '',
      sortOrder: 'asc',
      searchQuery: '',
      filteredSiswa: []
    }
  },
  computed: {
    sortedAndFilteredSiswa() {
      let result = [...this.filteredSiswa]
      
      if (this.sortKey) {
        result.sort((a, b) => {
          let valA = a[this.sortKey]
          let valB = b[this.sortKey]
          
          // Handle string comparison
          if (typeof valA === 'string') {
            valA = valA.toLowerCase()
            valB = valB.toLowerCase()
            return this.sortOrder === 'asc' 
              ? valA.localeCompare(valB)
              : valB.localeCompare(valA)
          }
          
          // Handle number comparison
          if (this.sortOrder === 'asc') {
            return valA - valB
          } else {
            return valB - valA
          }
        })
      }
      
      return result
    },
  },
  methods: {
    async getData() {
      try {
        const response = await axios.get(API_URL)
        this.siswa = response.data
        this.filteredSiswa = [...this.siswa]
      } catch (error) {
        console.error('Gagal mengambil data:', error)
      }
    },
    applySearch() {
      if (!this.searchQuery) {
        this.filteredSiswa = [...this.siswa]
        return
      }
      
      const query = this.searchQuery.toLowerCase()
      this.filteredSiswa = this.siswa.filter(siswa => 
        siswa.nama.toLowerCase().includes(query) ||
        siswa.alamat.toLowerCase().includes(query) ||
        siswa.umur.toString().includes(query)
      )
    },
    clearSearch() {
      this.searchQuery = ''
      this.filteredSiswa = [...this.siswa]
    },
    applySort() {
      // Sorting logic now handled in computed property
    },
    async hapusSiswa(id) {
      const confirm = await Swal.fire({
        title: 'Yakin?',
        text: 'Data akan dihapus permanen!',
        icon: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#d33',
        cancelButtonColor: '#3085d6',
        confirmButtonText: 'Ya, hapus!',
      })

      if (confirm.isConfirmed) {
        try {
          await axios.delete(`${API_URL}/${id}`)
          this.getData()
          Swal.fire('Dihapus!', 'Data siswa berhasil dihapus.', 'success')
        } catch (error) {
          console.error('Gagal menghapus data:', error)
        }
      }
    },
    editSiswa(siswa) {
      this.siswaEdit = { ...siswa }
    },
    toggleSortOrder() {
      this.sortOrder = this.sortOrder === 'asc' ? 'desc' : 'asc'
    },
    capitalizeWords(text) {
      return text.replace(/\b\w/g, (char) => char.toUpperCase())
    },
  },
  mounted() {
    this.getData()
  },
}
</script>

<style scoped>
.main-wrapper {
  background: linear-gradient(135deg, #2b8a3e, #40c057, #2b8a3e);
  background-size: 400% 400%;
  animation: gradientFlow 15s ease infinite;
  min-height: calc(100vh - 56px); /* Subtract navbar height */
  padding-top: 20px;
}

.main-header {
  background: linear-gradient(135deg, #2b8a3e, #40c057);
  padding: 1.5rem;
  border-radius: 10px;
  margin-bottom: 2rem;
  box-shadow: 0 4px 20px rgba(43, 138, 62, 0.3);
}

.form-section {
  background: linear-gradient(135deg, #2b8a3e, #40c057);
  padding: 1.5rem;
  border-radius: 10px;
  margin-bottom: 2rem;
  color: white;
}

.container {
  background-color: rgba(255, 255, 255, 0.95);
  border-radius: 15px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.table-header {
  background: linear-gradient(135deg, #2b8a3e, #40c057);
  color: white;
}

.table {
  background-color: rgba(255, 255, 255, 0.98);
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

.btn-outline-success {
  border-color: #2b8a3e;
  color: #2b8a3e;
}

.btn-outline-success:hover {
  background-color: #2b8a3e;
  color: white;
}

.btn-warning {
  background-color: #ffc107;
  border-color: #ffc107;
}

.btn-danger {
  background-color: #dc3545;
  border-color: #dc3545;
}

.input-group-text {
  background-color: #2b8a3e;
  border-color: #2b8a3e;
}

.dark-mode .table,
.dark-mode .table-header,
.dark-mode .table th,
.dark-mode .table td {
  background-color: var(--dark-card) !important;
  color: var(--dark-text) !important;
  border-color: var(--dark-border) !important;
}

.dark-mode .table th,
.dark-mode .table-header {
  background-color: var(--dark-hover) !important;
  color: var(--dark-text) !important;
}

.dark-mode .table-striped > tbody > tr:nth-of-type(odd) {
  background-color: #232323 !important;
}

.dark-mode .table-hover tbody tr:hover {
  background-color: #292929 !important;
  color: var(--dark-text) !important;
}

.dark-mode .btn-success,
.dark-mode .btn-outline-success {
  background-color: var(--dark-success) !important;
  border-color: var(--dark-success) !important;
  color: #fff !important;
  box-shadow: 0 2px 4px var(--dark-shadow) !important;
}

.dark-mode .btn-outline-success:hover,
.dark-mode .btn-success:hover {
  background-color: #157347 !important;
  border-color: #157347 !important;
  color: #fff !important;
}

.dark-mode .btn-outline-secondary {
  background-color: var(--dark-input) !important;
  border-color: var(--dark-border) !important;
  color: var(--dark-text) !important;
}

.dark-mode .btn-outline-secondary:hover {
  background-color: var(--dark-hover) !important;
  color: var(--dark-text) !important;
  border-color: var(--dark-success) !important;
}

.dark-mode .btn-success {
  background-color: var(--dark-success) !important;
  border-color: var(--dark-success) !important;
  color: #fff !important;
  box-shadow: 0 2px 4px var(--dark-shadow) !important;
}

.dark-mode .form-select {
  background-color: var(--dark-input) !important;
  color: var(--dark-text) !important;
  border-color: var(--dark-border) !important;
}

.dark-mode .form-select:focus {
  border-color: var(--dark-success) !important;
  box-shadow: 0 0 0 0.2rem rgba(25, 135, 84, 0.25) !important;
}

.dark-mode .btn-light,
.dark-mode button.btn-light {
  background-color: var(--dark-input) !important;
  color: #4caf50 !important;
  border-color: var(--dark-border) !important;
  box-shadow: 0 2px 4px var(--dark-shadow) !important;
}

.dark-mode .btn-light:hover,
.dark-mode button.btn-light:hover {
  background-color: var(--dark-hover) !important;
  color: #43d17a !important;
  border-color: var(--dark-success) !important;
}

.dark-mode .btn-outline-light,
.dark-mode button.btn-outline-light {
  background-color: transparent !important;
  color: #fff !important;
  border-color: var(--dark-border) !important;
}

.dark-mode .btn-outline-light:hover,
.dark-mode button.btn-outline-light:hover {
  background-color: var(--dark-hover) !important;
  color: #fff !important;
  border-color: var(--dark-success) !important;
}

.dark-mode .container[data-v-2dc54a20],
.dark-mode .container[data-v-2dc54a20] * {
  color: #4caf50 !important;
}

.dark-mode *:not(i) {
  color: #4caf50 !important;
}

/* Placeholder input */
.dark-mode input::placeholder,
.dark-mode textarea::placeholder {
  color: #4caf50 !important;
  opacity: 1 !important;
}

/* Tombol (button) */
.dark-mode button,
.dark-mode .btn,
.dark-mode .btn * {
  color: #4caf50 !important;
}

/* Select option */
.dark-mode select,
.dark-mode option {
  color: #4caf50 !important;
}
</style>