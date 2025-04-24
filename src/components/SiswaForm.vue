<template>
  <div class="card shadow-sm mb-4 border-0 bg-gradient">
    <div class="card-body p-4">
      <h2 class="text-center mb-4 text-white">
        <i :class="props.siswaEdit ? 'bi bi-pencil-square me-2' : 'bi bi-plus-circle me-2'"></i>
        {{ props.siswaEdit ? 'Edit Siswa' : 'Tambah Siswa' }}
      </h2>
      
      <form @submit.prevent="tambahAtauEditSiswa" class="row g-3 align-items-center">
        <div class="col-md-4">
          <div class="input-group">
            <span class="input-group-text bg-white"><i class="bi bi-person-fill text-primary"></i></span>
            <input
              v-model="nama"
              type="text"
              class="form-control"
              placeholder="Nama Siswa"
              required
            />
          </div>
        </div>
        
        <div class="col-md-4">
          <div class="input-group">
            <span class="input-group-text bg-white"><i class="bi bi-geo-alt-fill text-primary"></i></span>
            <input
              v-model="alamat"
              type="text"
              class="form-control"
              placeholder="Alamat"
              required
            />
          </div>
        </div>
        
        <div class="col-md-2">
          <div class="input-group">
            <span class="input-group-text bg-white"><i class="bi bi-calendar-event-fill text-primary"></i></span>
            <input
              v-model="umur"
              type="number"
              class="form-control"
              placeholder="Umur"
              required
            />
          </div>
        </div>
        
        <div class="col-md-2 d-grid">
          <button
            type="submit"
            class="btn btn-light text-primary fw-bold"
          >
            <i :class="props.siswaEdit ? 'bi bi-check-lg me-1' : 'bi bi-plus-lg me-1'"></i>
            {{ props.siswaEdit ? 'Update' : 'Tambah' }}
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'
import axios from 'axios'

const props = defineProps({
  siswaEdit: Object,
})

const emit = defineEmits(['siswa-added', 'cancel-edit'])

const nama = ref('')
const alamat = ref('')
const umur = ref(0)

watch(
  () => props.siswaEdit,
  (newSiswa) => {
    if (newSiswa) {
      nama.value = newSiswa.nama
      alamat.value = newSiswa.alamat
      umur.value = newSiswa.umur
    }
  },
  { immediate: true }
)

const resetForm = () => {
  nama.value = ''
  alamat.value = ''
  umur.value = 0
  emit('cancel-edit')
}

const API_URL = 'http://localhost:8081/siswa'

const tambahAtauEditSiswa = async () => {
  try {
    if (props.siswaEdit) {
      await axios.put(`${API_URL}/${props.siswaEdit.id}`, {
        nama: nama.value,
        alamat: alamat.value,
        umur: umur.value,
      })
    } else {
      await axios.post(API_URL, {
        nama: nama.value,
        alamat: alamat.value,
        umur: umur.value,
      })
    }

    resetForm()
    emit('siswa-added')
  } catch (err) {
    console.error('Gagal simpan:', err)
  }
}
</script>

<style scoped>
.bg-gradient {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}
</style>