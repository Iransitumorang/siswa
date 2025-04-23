<template>
    <form @submit.prevent="tambahSiswa">
      <input v-model="nama" placeholder="Nama" required />
      <input v-model="kelas" placeholder="Kelas" required />
      <button type="submit">Tambah</button>
    </form>
  </template>
  
  <script setup>
  import { ref } from 'vue'
  import axios from 'axios'
  
  const nama = ref('')
  const kelas = ref('')
  
  const emit = defineEmits(['siswa-added'])
  
  const tambahSiswa = async () => {
    try {
      await axios.post('http://localhost:8081/siswa', {
        nama: nama.value,
        kelas: kelas.value,
      })
      nama.value = ''
      kelas.value = ''
      emit('siswa-added')
    } catch (err) {
      console.error('Gagal tambah siswa:', err)
    }
  }
  </script>
  