<template>
    <div>
      <SiswaForm @siswa-added="fetchSiswa" />
  
      <h2>Daftar Siswa</h2>
      <ul v-if="siswaList.length">
        <li v-for="siswa in siswaList" :key="siswa.id">
          {{ siswa.nama }} ({{ siswa.kelas }})
          <button @click="hapusSiswa(siswa.id)">Hapus</button>
        </li>
      </ul>
      <p v-else>Belum ada siswa</p>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue'
  import axios from 'axios'
  import SiswaForm from '../components/SiswaForm.vue'
  
  const siswaList = ref([])
  
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
  
  onMounted(fetchSiswa)
  </script>
  