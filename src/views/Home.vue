<template>
  <div>
    <SiswaForm 
      :siswaEdit="siswaEdit" 
      @siswa-added="fetchSiswa" 
      @siswa-updated="handleSiswaUpdated" 
    />

    <h2>Daftar Siswa</h2>
    <table v-if="siswaList.length">
      <thead>
        <tr>
          <th>Nama</th>
          <th>Alamat</th>
          <th>Umur</th>
          <th>Aksi</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="siswa in siswaList" :key="siswa.id">
          <td>{{ siswa.nama }}</td>
          <td>{{ siswa.alamat }}</td>
          <td>{{ siswa.umur }}</td>
          <td>
            <button @click="hapusSiswa(siswa.id)">Hapus</button>
            <button @click="editSiswa(siswa)">Edit</button>
          </td>
        </tr>
      </tbody>
    </table>
    <p v-else>Belum ada siswa</p>
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

const handleSiswaUpdated = () => {
  siswaEdit.value = null
  fetchSiswa()
}

onMounted(fetchSiswa)
</script>