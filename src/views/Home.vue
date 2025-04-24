<template>
  <div class="container py-4">
    <div class="row justify-content-center">
      <div class="col-lg-10">
        <!-- Header Tengah -->
        <div class="text-center mb-5">
          <h1 class="display-5 fw-bold text-primary mb-3">
            <i class="bi bi-people-fill me-2"></i>
            Manajemen Siswa
          </h1>
          <p class="lead text-muted">
            Sistem pengelolaan data siswa dengan Vue.js dan Bootstrap 5
          </p>
        </div>

        <SiswaForm 
          :siswaEdit="siswaEdit" 
          @siswa-added="handleSiswaAdded"
          @cancel-edit="siswaEdit = null"
        />

        <div class="card shadow-sm border-0 mt-4">
          <div class="card-body p-0">
            <div class="table-responsive">
              <table class="table table-hover align-middle mb-0">
                <thead class="table-light">
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
                    <td>{{ siswa.nama }}</td>
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
.table-hover tbody tr:hover {
  background-color: rgba(13, 110, 253, 0.05);
}

.card {
  border-radius: 1rem;
}

.btn-sm {
  padding: 0.25rem 0.5rem;
  font-size: 0.875rem;
  border-radius: 0.5rem;
}
</style>