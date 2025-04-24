<template>
  <div class="min-h-screen bg-gradient-to-br from-indigo-100 to-white p-6">
    <div class="max-w-4xl mx-auto space-y-8">
      <SiswaForm :siswaEdit="siswaEdit" @siswa-added="handleSiswaAdded" />

      <div class="bg-white rounded-2xl shadow-xl p-6">
        <h2 class="text-3xl font-bold text-indigo-700 mb-6">📋 Daftar Siswa</h2>

        <table v-if="siswaList.length" class="w-full text-left border-separate border-spacing-y-4">
          <thead>
            <tr class="text-indigo-600 text-sm uppercase tracking-wider">
              <th>Nama</th>
              <th>Alamat</th>
              <th>Umur</th>
              <th>Aksi</th>
            </tr>
          </thead>
          <tbody>
            <tr
              v-for="siswa in siswaList"
              :key="siswa.id"
              class="bg-indigo-50 hover:bg-indigo-100 transition rounded-lg shadow-sm"
            >
              <td class="p-4 font-medium text-indigo-800">{{ siswa.nama }}</td>
              <td class="p-4">{{ siswa.alamat }}</td>
              <td class="p-4">{{ siswa.umur }}</td>
              <td class="p-4 space-x-2">
                <button
                  @click="editSiswa(siswa)"
                  class="px-4 py-1 rounded-full bg-yellow-400 hover:bg-yellow-500 text-white text-sm font-semibold shadow-sm transition"
                >
                  ✏️ Edit
                </button>
                <button
                  @click="hapusSiswa(siswa.id)"
                  class="px-4 py-1 rounded-full bg-red-500 hover:bg-red-600 text-white text-sm font-semibold shadow-sm transition"
                >
                  🗑️ Hapus
                </button>
              </td>
            </tr>
          </tbody>
        </table>

        <p v-else class="text-gray-600 italic">Belum ada siswa yang terdaftar.</p>
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
  try {
    await axios.delete(`http://localhost:8081/siswa/${id}`)
    fetchSiswa()
  } catch (err) {
    console.error('Gagal hapus siswa:', err)
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
