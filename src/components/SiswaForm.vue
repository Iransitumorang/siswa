<template>
  <div>
    <form @submit.prevent="isEditMode ? updateSiswa() : tambahSiswa()">
      <input v-model="formData.nama" placeholder="Nama" required />
      <input v-model="formData.alamat" placeholder="Alamat" required />
      <input v-model="formData.umur" type="number" placeholder="Umur" required />
      <button type="submit">{{ isEditMode ? 'Update' : 'Tambah' }}</button>
      <button v-if="isEditMode" type="button" @click="resetForm">Batal</button>
    </form>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'
import axios from 'axios'

const API_URL = 'http://localhost:8081/siswa'

const formData = ref({
  id: null,
  nama: '',
  alamat: '',
  umur: 0
})

const isEditMode = ref(false)

const emit = defineEmits(['siswa-added', 'siswa-updated'])

const props = defineProps({
  siswaEdit: {
    type: Object,
    default: null
  }
})

// Watch for changes in siswaEdit prop
watch(() => props.siswaEdit, (newVal) => {
  if (newVal) {
    isEditMode.value = true
    formData.value = { ...newVal }
  } else {
    resetForm()
  }
})

const tambahSiswa = async () => {
  try {
    await axios.post(API_URL, {
      nama: formData.value.nama,
      alamat: formData.value.alamat,
      umur: formData.value.umur,
    })
    resetForm()
    emit('siswa-added')
  } catch (err) {
    console.error('Gagal tambah siswa:', err)
  }
}

const updateSiswa = async () => {
  try {
    await axios.put(`${API_URL}/${formData.value.id}`, {
      nama: formData.value.nama,
      alamat: formData.value.alamat,
      umur: formData.value.umur,
    })
    resetForm()
    emit('siswa-updated')
  } catch (err) {
    console.error('Gagal update siswa:', err)
  }
}

const resetForm = () => {
  formData.value = {
    id: null,
    nama: '',
    alamat: '',
    umur: 0
  }
  isEditMode.value = false
}
</script>