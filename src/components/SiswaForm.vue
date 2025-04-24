<template>
  <div>
    <form @submit.prevent="tambahSiswa">
      <input v-model="nama" placeholder="Nama" required />
      <input v-model="alamat" placeholder="Alamat" required />
      <input v-model="umur" type="number" placeholder="Umur" required />
      <button type="submit">Tambah</button>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'

const API_URL = 'http://localhost:8081/siswa'

const nama = ref('')
const alamat = ref('')
const umur = ref(0)

const emit = defineEmits(['siswa-added'])

const tambahSiswa = async () => {
  try {
    await axios.post(API_URL, {
      nama: nama.value,
      alamat: alamat.value,
      umur: umur.value,
    })
    nama.value = ''
    alamat.value = ''
    umur.value = 0
    emit('siswa-added') // kasih tau parent untuk fetch ulang
  } catch (err) {
    console.error('Gagal tambah siswa:', err)
  }
}
</script>
