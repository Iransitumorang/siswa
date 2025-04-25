<template>
  <div class="main-wrapper py-4">
    <div class="container bg-white rounded shadow p-4">
      <header class="text-center mb-4">
        <h1 class="fw-bold text-success">CRUD DATA SISWA</h1>
        <p class="text-muted">Sistem manajemen data siswa menggunakan Vue.js dan Quarkus</p>
      </header>

      <SiswaForm
        :siswaEdit="siswaEdit"
        @siswa-added="getData"
        @cancel-edit="siswaEdit = null"
      />

      <div class="d-flex justify-content-end mb-3">
        <select v-model="sortKey" class="form-select w-auto me-2">
          <option value="">Sort By</option>
          <option value="nama">Nama</option>
          <option value="alamat">Alamat</option>
          <option value="umur">Umur</option>
        </select>
        <button class="btn btn-outline-success btn-sm" @click="toggleSortOrder">
          <i class="bi" :class="sortOrder === 'asc' ? 'bi-sort-alpha-down' : 'bi-sort-alpha-up'"></i>
        </button>
      </div>

      <div class="table-responsive">
        <table class="table table-bordered table-striped table-hover">
          <thead class="table-success text-center align-middle">
            <tr>
              <th>No</th>
              <th>Nama</th>
              <th>Alamat</th>
              <th>Umur</th>
              <th>Aksi</th>
            </tr>
          </thead>
          <tbody class="align-middle text-center">
            <tr v-for="(siswa, index) in sortedSiswa" :key="siswa.id">
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
            <tr v-if="sortedSiswa.length === 0">
              <td colspan="5">Belum ada data siswa</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Swal from 'sweetalert2'
import SiswaForm from '../components/SiswaForm.vue'

const API_URL = 'http://localhost:8081/siswa'

export default {
  components: {
    SiswaForm,
  },
  data() {
    return {
      siswa: [],
      siswaEdit: null,
      sortKey: '',
      sortOrder: 'asc',
    }
  },
  computed: {
    sortedSiswa() {
      if (!this.sortKey) return this.siswa
      return [...this.siswa].sort((a, b) => {
        let valA = a[this.sortKey]
        let valB = b[this.sortKey]
        if (typeof valA === 'string') valA = valA.toLowerCase()
        if (typeof valB === 'string') valB = valB.toLowerCase()

        if (this.sortOrder === 'asc') return valA > valB ? 1 : -1
        else return valA < valB ? 1 : -1
      })
    },
  },
  methods: {
    async getData() {
      try {
        const response = await axios.get(API_URL)
        this.siswa = response.data
      } catch (error) {
        console.error('Gagal mengambil data:', error)
      }
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
  background: linear-gradient(135deg, #1abc9c, #2ecc71, #27ae60);
  background-size: 400% 400%;
  animation: gradientFlow 15s ease infinite;
  min-height: 100vh;
  padding-top: 20px;
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
</style>

