<template>
  <div class="min-vh-100 bg-success-gradient">
    <div class="container py-5">
      <div class="row justify-content-center">
        <div class="col-lg-10">
          <!-- Header Tengah -->
          <div class="text-center mb-5 text-white">
            <h1 class="display-5 fw-bold mb-3">
              <i class="bi bi-people-fill me-2"></i>
              MANAJEMEN SISWA
            </h1>
            <p class="lead opacity-75">
              Sistem pengelolaan data siswa berbasis Quarkus dan Vue.js
            </p>
          </div>

          <SiswaForm 
            :siswaEdit="siswaEdit" 
            @siswa-added="handleSiswaAdded"
            @cancel-edit="siswaEdit = null"
          />

          <div class="card shadow-sm border-0 bg-white bg-opacity-90">
            <div v-if="siswaList.length">
              <div class="table-responsive">
                <table class="table table-hover align-middle mb-0">
                  <thead class="bg-success text-white">
                    <tr>
                      <th class="text-center">No</th>
                      <th>Nama</th>
                      <th>Alamat</th>
                      <th class="text-center">Umur</th>
                      <th class="text-center">Aksi</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="(siswa, index) in siswaList" :key="siswa.id">
                      <td class="text-center">{{ index + 1 }}</td>
                      <td class="fw-semibold">{{ siswa.nama }}</td>
                      <td>{{ siswa.alamat }}</td>
                      <td class="text-center">{{ siswa.umur }}</td>
                      <td class="text-center">
                        <button
                          @click="editSiswa(siswa)"
                          class="btn btn-sm btn-warning me-2"
                        >
                          <i class="bi bi-pencil-fill"></i>
                        </button>
                        <button
                          @click="hapusSiswa(siswa.id)"
                          class="btn btn-sm btn-danger"
                        >
                          <i class="bi bi-trash-fill"></i>
                        </button>
                      </td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </div>

            <div v-if="!siswaList.length" class="text-center py-5">
              <i class="bi bi-emoji-frown display-5 text-muted"></i>
              <p class="mt-3 text-muted">Belum ada data siswa</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import SiswaForm from '../components/SiswaForm.vue'

const siswaList = ref([])
const siswaEdit = ref(null)

const fetchSiswa = async () => {
  try {
    const res = await axios.get('http://localhost:8081/siswa')
    siswaList.value = res.data
  } catch (err) {
    console.error('Gagal ambil data:', err)
  }
}

const hapusSiswa = async (id) => {
  if (confirm('Apakah Anda yakin ingin menghapus siswa ini?')) {
    try {
      await axios.delete(`http://localhost:8081/siswa/${id}`)
      fetchSiswa()
    } catch (err) {
      console.error('Gagal hapus siswa:', err)
    }
  }
}

const editSiswa = (siswa) => {
  siswaEdit.value = siswa
}

const handleSiswaAdded = () => {
  fetchSiswa()
  siswaEdit.value = null
}

onMounted(fetchSiswa)
</script>

<style scoped>
.bg-success-gradient {
  background: linear-gradient(135deg, #198754 0%, #2a9d8f 100%);
}

.table-hover tbody tr:hover {
  background-color: rgba(40, 167, 69, 0.1);
}

.card {
  border-radius: 12px;
  overflow: hidden;
}

thead {
  background: linear-gradient(135deg, #20c997 0%, #198754 100%);
}

.btn-sm {
  padding: 0.25rem 0.5rem;
  font-size: 0.875rem;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}
</style>